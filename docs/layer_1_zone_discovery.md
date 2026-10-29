# Layer 1 — GEX Zone Discovery and History

Status: **Frozen design, version 1.0 — 2026-10-29.**

This is the authoritative Layer 1 contract for `spx-gex-agent`. It supersedes the Layer 1 geometry, data-quality and lifecycle proposals in `ARCHITECTURE.md`. Frozen means agreed for the first research version; it does not mean implemented, empirically validated or approved for live execution. Changes require an explicit revision of this document.

## 1. Purpose and boundaries

Layer 1 discovers spatial concentrations of modeled option gamma, describes their composition, and preserves what was known when. Layer 2 establishes price-reaction roles using completed candles.

| Input or observation | Responsibility |
|---|---|
| Modeled gross sensitivity | Locate candidate zones |
| Modeled net sensitivity | Describe call/put composition |
| Completed price reactions | Establish observed support, resistance or failed-level behavior in Layer 2 |

Layer 1 does not infer dealer holdings, guarantee pinning, issue BUY/SELL decisions, calculate a probability of success, or place orders. It does not impose the existing ORB higher-high/lower-low gate. The existing `spx_orb` application remains separate.

The first research comparison uses independent 1-minute and 5-minute reaction engines consuming the same zone versions. No future 5-minute close may validate or cancel an earlier 1-minute decision retrospectively.

## 2. Universe and measurement contract

Each snapshot identifies its underlying, contract roots, exact expiration set, OI baseline, model version and configuration. All measures in a snapshot use the same universe. Expirations or model variants must never be silently mixed.

The chosen provider, contract universe and valuation implementation must be documented and verified before implementation is accepted. This contract does not assert that a particular official downloadable OI file is available through the current subscription.

GEX values use dollar sensitivity per 1% underlying move, with assumed positive call signs and negative put signs. These signs are modeling assumptions, not observed dealer positions.

For strike K:

```text
call_gex[K] >= 0
put_gex[K]  <= 0
gross[K] = abs(call_gex[K]) + abs(put_gex[K])
net[K]   = call_gex[K] + put_gex[K]
```

For cluster C:

```text
gross[C] = sum(gross[K] for K in C)
net[C]   = sum(net[K] for K in C)
bias[C]  = net[C] / gross[C]  # defined only when gross[C] > 0
```

| Bias | Composition label |
|---|---|
| Greater than +0.35 | `CALL_DOMINANT` |
| Less than -0.35 | `PUT_DOMINANT` |
| Inclusive interval [-0.35, +0.35] | `BALANCED` |

The metric is **modeled gross sensitivity**, not capital committed to defending a level or an upper bound on actual dealer exposure. Balanced means balanced modeled sensitivities; it does not imply equal OI, known offsetting positions or a pin. Composition labels never assign a support/resistance role.

Retain missing-side and omitted-contract evidence alongside computed measures. A label calculated from usable contracts must be identified as such; missing contracts must not be represented as measured zero exposure.

## 3. Corridor, baseline and completeness

Use an inclusive strike corridor `[spot - 75.0, spot + 75.0]`, where spot is the causal SPX input used for that snapshot. Persist its price, timestamps and both corridor bounds. The width is fixed for version 1; the corridor moves with snapshot spot. It is not an ATR-based or session-fixed window.

The independent baseline contains prior-session reported OI for the configured universe. It must be established separately from the quote response and retain provider, report/session date, retrieval time, contract identities and completeness evidence. A missing or unverifiable baseline is unavailable data, never zero OI. The baseline must cover the current corridor even as spot moves.

```text
baseline_oi = sum(prior_session_oi for all baseline corridor contracts)
usable_oi   = sum(prior_session_oi for usable baseline corridor contracts)
coverage    = usable_oi / baseline_oi
```

Contracts with reported OI > 0 contribute to the baseline denominator. A usable contract must have:

- A quote no more than 120 seconds old at snapshot valuation time, with no future timestamp.
- Finite prices, a non-negative bid and a non-crossed spread (`ask >= bid`).
- A valid price under the documented valuation policy and solvable annualized IV in the inclusive range `[0.01, 3.00]`.
- Valid finite gamma computed from causal inputs for that contract.

A zero bid does **not** establish zero value or zero gamma. If its own quote supports valid valuation, use the resulting sensitivity. Otherwise mark that contract unusable. Do not copy Greeks from the opposite option side or manufacture zero exposure. These are GEX-input rules, separate from any later tradable-contract bid floor.

Usable contracts at a partially covered strike may contribute measured sensitivity; omitted positive-OI contracts count against coverage and remain explicit in zone metadata. Numerator and denominator use identical valuation and inclusion criteria.

| Condition | Result |
|---|---|
| Required baseline, spot or model inputs unavailable | `DATA_UNAVAILABLE`; no new zones |
| Baseline OI > 0 and coverage < 90% | `DATA_INCOMPLETE`; no new zones |
| Independently verified baseline OI = 0 | `ZERO_BASELINE_OI`; no zones; coverage undefined |
| Coverage >= 90% but valid modeled gross total = 0 | `ZERO_MODELED_EXPOSURE`; no zones; dominance undefined |
| Valid positive gross total and coverage >= 90% | Discover zones |

Neither zero status asserts absence of actual intraday dealer exposure. Complete data with no cluster above the candidate threshold is valid diffuse concentration, not a data failure. It produces no eligible zone-based setup.

## 4. Deterministic greedy clustering

Cluster positive-gross usable strikes inside the corridor. Preserve other measurements and exclusions in storage. Adjacency in this version means an exact 5-point strike difference; do not bridge a missing or unusable strike or presume every possible strike was listed.

1. Rank unassigned strikes by gross sensitivity descending, then absolute distance to snapshot spot ascending, then strike ascending.
2. Select the highest-priority unassigned strike as the seed.
3. Inspect only present, unassigned neighbors at `seed - 5` and `seed + 5`.
4. A neighbor qualifies when its gross sensitivity is at least 50% of the seed's.
5. Absorb at most one neighbor: prefer larger gross, then smaller distance to snapshot spot, then the lower strike.
6. Mark the seed and any absorbed neighbor assigned; repeat until the pool is empty.

Each strike belongs to exactly one discovered cluster, and each cluster contains one or two strikes. Equal-height plateaus are handled by the same ordering; no strict local-maximum test is required. Replay uses identical numeric comparison and tie-breaking rules.

```text
lower = minimum_member_strike - 2.50
upper = maximum_member_strike + 2.50
```

A singleton is 5 points wide. Two strikes spaced 5 points apart form a 10-point zone. Three-strike clusters are excluded. These are configurable research parameters whose values are recorded with each snapshot; changing them creates a new configuration version.

## 5. Dominance and candidate eligibility

```text
dominance[C] = gross[C] / sum(gross[K] for all valid corridor strikes)
```

The denominator includes sub-threshold clusters. Each strike contributes once. The corridor applies to member strikes, not the padded band edges; a band's padding may extend beyond the corridor.

| Dominance | Label | New setup eligibility |
|---|---|---|
| >= 20% | `TIER_1_CONCENTRATION` | Eligible for Layer 2 evaluation |
| >= 10% and < 20% | `TIER_2_CONCENTRATION` | Eligible for Layer 2 evaluation |
| < 10% | `SUB_THRESHOLD_INVENTORY` | Stored; excluded from active candidate initiation |

These tiers are concentration labels, not probabilities or validated quality scores. No 25-point score allocation, first-touch guarantee, touch-fatigue multiplier or net-GEX directional bonus is established here. Save all clusters and source bars so alternate thresholds can later be evaluated causally.

## 6. Session phases

Use the applicable market session calendar and America/New_York time. The normal session schedule is:

- **09:30–10:00:** collect snapshots and price data, discover zones and log causal observations. Entry candidates are disabled.
- **From 10:00 until session close:** permit research candidate evaluation when all required conditions are valid. Execution cutoffs are a separate contract.

Start collection at the open and refresh at five-minute ET boundaries. Actual availability controls eligibility; do not assume the first snapshot exists precisely at 09:30. Missing startup history is marked incomplete, not reconstructed from a later GEX snapshot.

Morning observations can inform later history features. Observation-only setups do not automatically become actionable at 10:00. Layer 2 must require a qualifying confirmation after the observation window and define setup eligibility at that boundary. Session end stops new research candidates and closes the session's observation lifecycle; it does not prescribe a trade fill.

## 7. Causal timing and freshness

Persist the following clocks separately:

| Clock | Meaning |
|---|---|
| Per-contract `source_ts` | Quote's source timestamp |
| `snapshot_asof_ts` | Valuation cutoff used for spot, quotes and model inputs |
| `available_ts` | Time ingestion, validation and clustering completed |
| Bar start/end and availability | Candle interval and when the completed input became available |

Quote freshness is checked against `snapshot_asof_ts`, not each later candle close. Snapshot eligibility expires when `decision_ts - snapshot_asof_ts > 360 seconds`. Exactly 360 seconds remains within TTL. Processing delay consumes TTL; it never extends it.

Only snapshots available by a decision may influence it. A snapshot available at 10:05:18 cannot be queried at 10:05:01 or used to change the already completed 10:00–10:05 decision.

For full-candle touch, wick and traversal evidence, the bound zone must have been available no later than that candle's opening time. With availability at 10:05:18, the first fully eligible ordinary 1-minute candle starts at 10:06; the first fully eligible 5-minute candle starts at 10:10. An older eligible zone may still support an earlier candle.

Historical replay preserves recorded availability, data gaps and configuration. Reconstructed snapshots without historical availability evidence must be labeled retrospective research; they cannot establish what a live engine knew. Late corrections create new records and do not rewrite prior decisions.

## 8. Frozen setups and refresh failures

An eligible interaction binds a Layer 2 setup to one immutable zone version. Boundaries, initial dominance, initial composition and input provenance remain frozen for that setup. New snapshots create new versions; they cannot move an existing trigger boundary.

Current data health is separate from frozen geometry. A stale snapshot, missing required input or incomplete refresh blocks new setup initiation and entry confirmation. Retaining an old successful snapshot must not silently bypass a newer known data failure.

An active setup may continue logging price observations against its frozen zone during a data failure. A blocked trigger is recorded as such; it is not queued for automatic execution. After valid data returns, a fresh qualifying confirmation is required, subject to Layer 2 expiry and invalidation rules. Old geometry alone never establishes current data validity.

Changes in current rank or composition are recorded without rewriting the setup's original features. Any additional rule requiring continued concentration or composition alignment belongs to a later explicitly defined entry policy.

## 9. Identity, lineage and event history

| Entity | Immutable identity and purpose |
|---|---|
| Snapshot | Session, universe, model/configuration, valuation cutoff and observation/revision identity |
| Zone version | Snapshot identity plus exact member strikes and geometry |
| Level history | Session-local history linking related zone versions |
| Setup | Timeframe-specific interaction bound to one zone version |
| Interaction event | Original observed event with its time, zone, setup, timeframe and supporting bars |

The seed is an attribute, not the sole permanent identity. Link successive versions using explicit shared-strike lineage, retaining predecessor/successor relationships for merges and splits. No shared member strike means no automatic inheritance. Lineage does not assert unchanged geometry or unchanged price role.

Maintain separate views of:

- Events observed against the exact zone geometry.
- Events associated with related earlier versions or constituent strikes.

Never deduplicate by taking the maximum counter or blindly summing counters. Derived counts use distinct immutable event identities within an explicit history scope. Keep 1-minute and 5-minute histories separate; simultaneous observations are not automatically two independent market tests.

Splits do not rewrite past cluster rejections as newly observed singleton rejections. Historical bars may support a separately labeled research reclassification, but original events and their availability remain unchanged. Earlier decisions cannot use later lineage or reclassification knowledge.

Layer 2 defines what constitutes a distinct test, when it ends, re-arming, reaction labels and how history affects scoring. Layer 1 provides the event provenance and lineage, not an assumed fatigue counter.

## 10. Spatial ownership and bar evidence

Use half-open intervals `[lower, upper)` for point membership. A shared boundary belongs to the upper adjacent zone.

Point membership and exterior boundary testing are distinct. A price arriving from above and touching a lower zone's upper boundary can be recorded as testing that boundary even though the exact point belongs to the upper zone. Layer 2 must choose a deterministic interaction owner; membership alone must not emit duplicate rejection events.

A candle overlapping multiple bands is recorded with `MULTI_ZONE_SPAN` evidence. High and low do not reveal intrabar visit order or prove an A-to-B transition. Previous close, open and close may support directional observations, but cannot fabricate the missing path. A span alone increments no clean rejection count.

## 11. Persistence and restart contract

Persist source inputs or reproducible references, OI baseline provenance, contract exclusions, quote timestamps, snapshots, all zone versions, lineage and append-only interaction events. Retain explicit incomplete-history and data-health states. Snapshot identity cannot be only a wall-clock timestamp.

Identical ingestions and replays must be idempotent. Distinct sources, configurations and legitimate revisions must remain distinguishable. Restart reconstructs state from stored causal events and inputs, preserving incomplete periods; it must not recount every candle as a new touch. Atomic persistence must prevent partially published snapshots or events from becoming eligible inputs.

Forward returns, MFE/MAE and outcome classifications belong in separate evaluation storage. Entry evaluation interfaces must not receive those fields. Table separation alone is insufficient if the decision path can still read future data.

## 12. Layer 2 handoff and acceptance examples

Layer 1 supplies versioned bounds, member strikes, seed, gross/net/bias, composition and completeness metadata, dominance tier, valuation/availability times, TTL/data-health state, session phase and causal history references.

Layer 2 owns rejection and break/retest definitions, approach conditions, distinct tests, re-arming, setup expiry, role changes, candle confirmations and any RVOL interpretation. Subsequent documents own regime scoring, combined quality scores and execution. None is implicitly finalized here.

Required implementation checks include:

1. +3B call and -3B put sensitivity produce 6B gross, zero net and balanced composition.
2. Equal-gross seeds and competing neighbors yield repeatable, non-overlapping strike assignments.
3. A 4B seed absorbs a 2B neighbor at the inclusive 50% boundary; no third strike is absorbed.
4. Missing quote contracts remain in the independently established OI denominator; exactly 90% passes coverage.
5. Zero bid is never automatically converted to zero gamma; missing baseline is never zero exposure.
6. A 10:05:18 snapshot cannot influence a 10:05 decision or claim the earlier portion of a forming candle.
7. TTL allows exactly 360 seconds and blocks greater ages; an incomplete refresh cannot silently fall back to healthy status.
8. A blocked trigger is not released automatically after recovery.
9. Refreshes, seed changes, merges and splits preserve original events and frozen setup boundaries.
10. Restart and repeated ingestion produce the same event identities and no duplicate counts.
11. Exact boundary ownership is deterministic, and multi-zone spans do not invent an intrabar path.
12. Sub-threshold clusters are retained; independent 1m/5m evaluation cannot access future outcomes.

## References and limitations

- [Cboe: Evaluating the Market Impact of SPX 0DTE Options](https://www.cboe.com/insights/posts/volatility-insights-evaluating-the-market-impact-of-spx-0-dte-options): actual net positions matter for hedging; gross activity is not equivalent to dealer hedging demand.
- [OIC: Gamma](https://www.optionseducation.org/advancedconcepts/gamma): gamma describes delta sensitivity; a bid quote alone does not establish gamma.

The numerical thresholds in this document are research defaults, not conclusions established by these sources. Provider access, valuation details and input-quality implementation require verification before software acceptance; predictive value requires separate empirical testing.
