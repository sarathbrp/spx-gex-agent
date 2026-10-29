# Signal Quality Engine — Proposed Architecture

Status: Draft for discussion; not implemented or validated.

Frozen specifications (version 1.0, 2026-10-29):

- [Layer 1 — GEX Zone Discovery and History](docs/layer_1_zone_discovery.md) supersedes the original Layer 1 and overlapping timing, data-quality and zone-history proposals below.
- [Layer 2 — Price Reactions and Setup Evaluation](docs/layer_2_price_reactions.md) supersedes the original reaction, episode/test counting, breakout/retest and confirmation proposals below. It defines pattern detection and candidate eligibility; the original reaction-score allocations and fatigue multipliers are not frozen requirements.
- [Layer 3 — Environmental Context](docs/layer_3_environmental_context.md) freezes broad/ex-zone GEX, VIX/VIX1D and SPY RVOL observations. Missing or conflicting Layer 3 context never blocks an eligible Layer 2 candidate; the original regime bonuses and volatility vetoes below are superseded.
- [Layer 4 — Multi-Timeframe Alignment](docs/layer_4_multitimeframe_alignment.md) freezes observation-only 1m/5m matching, immutable decision-time states and subsequent comparison events. It supersedes the original alignment bonus, forming-candle trap veto and retrospective confirmation proposals below.
- [Persistent State Machine and Storage Schema](docs/persistent_state_and_storage.md) freezes SQLite on local storage, one engine writer, multiple read-only viewers, durable event history, atomic transitions and causal recovery.

Layers 1–4 and persistence are frozen in the linked specifications. Combined scoring, validation and execution proposals below remain discussion drafts. Where this original proposal conflicts with a frozen specification, the frozen document takes precedence.

The original three-table storage sketch below is retained as proposal history and is superseded by the frozen persistence specification. Executable migrations and application code have not been implemented by these design documents.

Implementation work is organized in the [task checklist](docs/TASKS.md), with one document per task and a [requirements traceability checklist](docs/tasks/TRACEABILITY.md). Implementation, test, acceptance and review statuses start unchecked; the plan does not imply completed code or validation.

A robust **Signal Quality Engine (SQE)** for `spx-gex-agent` must bridge the gap between static open-interest levels and dynamic order flow. Instead of treating every zone touch as an equal trade trigger, the engine should evaluate candidate setups across four distinct scoring layers, producing a composite **Signal Quality Score (SQS: 0–100)**.

---

### Layer 1: Zone Geometry & Structural Gravity ($S_{\text{zone}}$, 25%)

A raw strike level is a single price; an institutional entry zone is an area of localized delta/gamma friction.

#### 1. Zone Width Clustering

* SPX strikes trade in 5-point increments. When adjacent strikes have high gross gamma (e.g., 7695 Put and 7700 Put), evaluate them as a **composite band** rather than two conflicting levels:



$$\text{Zone}_{\text{Lower}} = K_{\text{base}} - 1.5\text{ pts}, \quad \text{Zone}_{\text{Upper}} = K_{\text{base}} + 1.5\text{ pts}$$



*For a cluster (e.g., 7695–7700):* Zone = $[7693.50, 7701.50]$.


* **Strike Proximity Penalty:** If two competing walls sit within 10 points of each other (e.g., 7725 support vs. 7730 resistance), cap $S_{\text{zone}}$ due to tight range compression.

#### 2. Relative Gamma Dominance

Score the zone by its relative share within the local $\pm 100$-point window:


$$\text{Dominance Ratio} = \frac{\vert{}\text{GEX}_{\text{zone}}\vert{}}{\sum_{i \in \text{Local}} \vert{}\text{GEX}_i\vert{}}$$

* **Score $\ge 0.20$:** 25/25 pts (Dominant Wall)
* **Score $0.10 - 0.19$:** 15/25 pts (Secondary Wall)
* **Score $< 0.10$:** 0/25 pts (Dispersed / Low Relevance)

---

### Layer 2: Kinematic Price Reaction Scoring ($S_{\text{reaction}}$, 35%)

Rather than relying on visual interpretation, define the four core price behaviors deterministically using bar geometry and relative volume ($\text{RVOL}$):

```
          REJECTION (Fade Setup)                        RETEST & HOLD (Breakout Setup)
       ─────────────────────────────                 ────────────────────────────────────
             [ Upper Wick >= 40% ]                                   [ Expansion Candle ]
                   ┌──┐                                                    ┌──┐
                   │  │                                                    │  │
                   └──┘                                                    └──┘
       ═════════════════════════════  Zone Top       ════════════════════════════════════  Zone Top
       ─────────────────────────────  Zone Bottom    ────────────────────────────────────  Zone Bottom
                   ┌──┐                                                    ▲
                   │  │                                          [ Pullback Wick Rejection ]
                   └──┘
            [ Low Penetration Only ]

```

#### Deterministic Setup Definitions

| Setup Type | Mathematical Criteria | Candidate Signal |
| --- | --- | --- |
| **Support Rejection** | 1. Bar Low enters Zone, but Bar Close $\ge \text{Zone}_{\text{Upper}}$.<br>

<br>2. Lower wick $\ge 40\%$ of full bar range $(H - L)$.<br>

<br>3. Close in the upper third of the bar range. | **CALL** |
| **Resistance Rejection** | 1. Bar High enters Zone, but Bar Close $\le \text{Zone}_{\text{Lower}}$.<br>

<br>2. Upper wick $\ge 40\%$ of full bar range.<br>

<br>3. Close in the lower third of the bar range. | **PUT** |
| **Breakout + Retest** | 1. Bar $t-k$ closes completely outside zone boundary by $\ge 0.5 \times \text{ATR}_{15\text{m}}$.<br>

<br>2. Pullback bar $t-1$ or $t$ penetrates zone boundary by $\le 2.0$ points without closing back inside.<br>

<br>3. Current bar closes in breakout direction with $\text{RVOL} \ge 1.3$. | **CALL / PUT** |
| **Rejection-then-Break** | 1. Strike records $1 - 2$ prior valid rejections.<br>

<br>2. Subsequent bar forces a full close beyond the zone with displacement $\ge 1.0 \times \text{ATR}$.<br>

<br>3. No wick rejection printed against the breakout direction. | **CALL / PUT** (Momentum) |

#### Touch Fatigue Degradation

In auction market theory, each successive retest absorbs localized liquidity:

* **Touch 1:** Maximum probability of rejection ($1.0 \times$ score multiplier).
* **Touch 2:** Neutral ($0.8 \times$ multiplier).
* **Touch 3+:** High probability of breakthrough / failure ($0.4 \times$ multiplier for fades; boost breakout score).

---

### Layer 3: Regime & Environmental Alignment ($S_{\text{regime}}$, 25%)

The same price reaction has completely different expected value depending on aggregate Net GEX and macro momentum:

#### 1. Net GEX Bias Multiplier

* **Positive Net GEX Regime ($\text{Net GEX} > +1.5\text{B}$):**
* Support / Resistance Rejections (Fades): **$+15$ bonus points**.
* Breakout / Retest Setups: **$-20$ penalty points** (or hard veto if $> +3.0\text{B}$).


* **Negative Net GEX Regime ($\text{Net GEX} < -1.5\text{B}$):**
* Breakout / Retest Setups: **$+15$ bonus points** (dealers accelerate moves).
* Fades: **$-15$ penalty points** (high risk of getting steamrolled).



#### 2. Intraday VIX Slope ($\Delta \text{VIX}$)

* **For CALL candidates:** VIX 5-minute linear regression slope must be $\le 0$. If VIX is rising rapidly during an upward test, dock 10 points (hedging demand divergence).
* **For PUT candidates:** VIX 5-minute slope must be $\ge 0$.

---

### Layer 4: Multi-Timeframe Alignment ($S_{\text{confluence}}$, 15%)

Because the engine evaluates 1-minute and 5-minute candles independently, their interaction serves as an internal cross-validation filter:

* **Confluent Agreement (1m + 5m):** A 1-minute rejection candle occurs at the close of an active 5-minute candle that is also rejecting the zone $\implies$ **15/15 pts**.
* **Micro-Break / Macro-Wick Trap:** A 1-minute bar closes outside the zone (registering a "break" on the 1m engine), but the corresponding 5-minute bar is mid-flight and ultimately closes back inside as a long wick $\implies$ Flagged as **`FALSE_BREAK_TRAP`** (auto-veto 1m breakout signal).
* **Early 1m Entry into 5m Base:** 1-minute forms a higher-low base inside the zone while 5-minute consolidates $\implies$ Yields early entry for research comparison with tighter stop placement.

---

### Composite SQS Matrix & Execution Tiers

$$\text{SQS} = S_{\text{zone}} + S_{\text{reaction}} + S_{\text{regime}} + S_{\text{confluence}} \quad (\text{Scale: } 0 - 100)$$

| Tier | SQS Range | Action in Replay / Paper Engine |
| --- | --- | --- |
| **Tier 1 (High Quality)** | **$80 - 100$** | Validated trade signal. Execute paper contract selection and track PnL. |
| **Tier 2 (Research Only)** | **$60 - 79$** | Record signal and track theoretical path, but do not execute paper order. Flag specific deficient factor. |
| **Tier 3 (Sub-Par / Veto)** | **$< 60$** | Suppressed / Discarded. Log reason (e.g., `TOUCH_FATIGUE`, `REGIME_CONFLICT`). |

---

### Persistent State Machine & Storage Schema

To ensure the agent survives mid-day crashes or restarts without losing morning context, decouple state into three lightweight SQLite / PostgreSQL tables:

```
┌─────────────────────────┐       ┌─────────────────────────┐       ┌─────────────────────────┐
│     gex_snapshots       │       │       zone_registry     │       │     zone_reactions      │
├─────────────────────────┤       ├─────────────────────────┤       ├─────────────────────────┤
│ timestamp (PK)          │ 1   * │ zone_id (PK)            │ 1   * │ reaction_id (PK)        │
│ spot_price              │──────<│ strike_center           │──────<│ zone_id (FK)            │
│ net_gex                 │       │ zone_lower / upper      │       │ timeframe (1m / 5m)     │
│ raw_chain_json          │       │ gex_type (CALL/PUT/NET) │       │ reaction_type           │
│ source_coverage_ratio   │       │ established_at          │       │ test_count              │
└─────────────────────────┘       │ dominance_share         │       │ sqs_score               │
                                  └─────────────────────────┘       │ outcome_forward_bars    │
                                                                    └─────────────────────────┘

```

1. **`gex_snapshots`**: Stores periodic snapshots, aggregate Net GEX, and raw contract coverage metrics.
2. **`zone_registry`**: Stores discovered zones created strictly at 10:00 ET (or during macro updates), keeping established timestamps immutable so future price action cannot contaminate zone identification.
3. **`zone_reactions`**: Maintains an append-only log of every bar interaction (`TEST`, `REJECT_UP`, `REJECT_DOWN`, `BREAK_UP`, `BREAK_DOWN`, `RETEST_HOLD`). Tracks `test_count` cumulatively so restarts instantly reload accurate touch counts.
