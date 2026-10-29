# Layer 2 — Price Reactions and Setup Evaluation

Status: **Frozen design, version 1.0 — 2026-10-29.**

This specification follows [Layer 1 — GEX Zone Discovery and History](layer_1_zone_discovery.md) and supersedes the original reaction, touch-fatigue and confirmation proposals in `ARCHITECTURE.md`. It incorporates the agreed immediate rejection confirmation, episode indexing and next-candle retest baseline, with explicit causality and persistence safeguards.

Frozen means the first research contract is agreed, not that the application is implemented or the strategy is profitable. Changes require a documented version revision. No existing `spx_orb` behavior is changed.

## 1. Responsibility and outputs

Layer 2 classifies completed price interactions with previously available, frozen GEX zones. It reports the evidence and whether a pattern meets this layer's candidate-eligibility requirements. It does not select option contracts, imply fills, assign trade quality probabilities, or place orders.

| Setup | Observed behavior | Candidate direction |
|---|---|---|
| `SUPPORT_REJECTION` | Approach from above; touch and recover above the zone with qualifying candle geometry | CALL |
| `RESISTANCE_REJECTION` | Approach from below; touch and recover below the zone with qualifying candle geometry | PUT |
| `UPWARD_BREAK_RETEST` | Fresh buffered break upward, followed by a later qualifying retest and hold | CALL |
| `DOWNWARD_BREAK_RETEST` | Fresh buffered break downward, followed by a later qualifying retest and hold | PUT |

Prior rejections are historical evidence, not a separate rejection-then-break entry strategy. Composition labels never determine setup direction. RVOL, VIX, net-GEX regime adjustments, fatigue multipliers and combined scores remain outside this frozen layer. A candidate is an input to subsequent quality and execution policies, not an instruction to execute.

## 2. Input and time contract

Use separate deterministic engines for 1-minute and 5-minute SPX candles. Five-minute buckets start at 09:30 ET and require every constituent minute. Only complete, valid OHLC bars may drive state changes. Require finite prices, `low <= min(open, close) <= max(open, close) <= high`, correct session alignment and no conflicting duplicates.

All times have an explicit timezone. Record candle start/end, candle availability and actual evaluation/decision time. A close-time observation is only emitted when the completed bar becomes available; it is never backdated into an executable signal.

For a new episode, use the most recent eligible zone version that was available no later than the tested candle's opening. Its causal origin close must also be known before that candle opens. Freeze the zone for the entire episode. Later snapshots may inform current data health, but cannot supply retrospective geometry or move an active boundary.

Layer 1's snapshot TTL, coverage, availability and current data-health gates apply independently of frozen setup geometry. Historical replay observes recorded availability and never substitutes subsequently reconstructed data into an earlier decision.

## 3. Research parameters

| Parameter | 1-minute engine | 5-minute engine |
|---|---|---|
| Minimum rejection range | 0.50 SPX points | 0.50 SPX points |
| Minimum directional wick fraction | 0.40 | 0.40 |
| Minimum directional close fraction | Exact 2/3 | Exact 2/3 |
| Consecutive inside-close stall limit M | 10 | 5 |
| Re-arm duration N | 3 completed bars | 1 completed bar |
| Re-arm displacement D | `max(3.0, 0.35 * frozen_ATR)` | Same |
| Break displacement E | `max(2.0, 0.20 * frozen_ATR)` | Same |
| Earliest retest | Bar immediately after breakout | Same |
| Latest retest | 15 bars after breakout | 6 bars after breakout |
| Retest exterior proximity | 1.0 point | 1.0 point |
| Maximum retest penetration | 50% of frozen zone width | Same |

All values are configurable research defaults, persisted with a configuration version. Different timeframe limits represent different elapsed durations: comparisons must identify both timeframe and timeout policy. No superiority or frequency claim about immediate retests is assumed.

## 4. ATR definition and freezing

Use 14-period Wilder ATR from completed 15-minute SPX RTH candles, anchored to the session open. For each true range:

```text
TR[i] = max(H[i] - L[i], abs(H[i] - previous_close), abs(L[i] - previous_close))
```

For the first RTH candle of a session, `previous_close` is the documented preceding-session official cash close; otherwise it is the previous completed 15-minute candle's close. Retain source identity and availability for the prior close. Never silently replace it with a different close convention. Overnight movement is included in the first candle's true range; synthetic overnight candles are not created.

To make the proposed rolling warm-up reproducible, each ATR reference uses the most recent 130 scheduled, completed 15-minute bars plus the preceding close needed for its first true range. Seed with the arithmetic mean of the first 14 true ranges; apply `(13 * previous_ATR + current_TR) / 14` to each remaining true range. Persist the window bounds and reference bar time. The window rolls deterministically; it is not an unrecorded process-start seed.

130 bars correspond to five normal full sessions, not necessarily five calendar days. Holidays and early closes follow the session calendar; scheduled closures are not missing bars. Missing expected bars, fewer than 130 usable bars, unavailable prior-close inputs or non-finite results produce `ATR_UNAVAILABLE`. Do not skip holes or fall back silently to point floors.

At episode initiation, select an ATR reference available by the trigger candle's opening and freeze its value, reference times and derived D/E thresholds. This prevents the triggering candle from changing its own thresholds. A breakout watch inherits its parent episode's frozen geometry, ATR and thresholds; spawning the watch does not recalculate the breakout test.

An existing episode with a valid frozen ATR can finish observing geometry during later feed problems, but current data eligibility still applies. Missing ATR prevents new episode/setup generation; raw price observations may still be retained.

## 5. Episode identity and indexing

An episode is one encounter with a zone, potentially spanning multiple candles. It is separate from a completed rejection, a breakout watch and an eligible candidate.

```text
READY -> TESTING -> RESOLVED -> RE_ARMING -> READY
                   (including STALL)
```

Create the episode and assign its test index exactly once on `READY -> TESTING`, when a completed candle first establishes the touch. Store the candle interval; do not claim an exact intrabar touch timestamp from OHLC. No counter is incremented at resolution or re-arming.

The exact-zone test index is scoped to session, timeframe, configuration and frozen member-strike/geometry identity, with explicit observed-history completeness. Repeated versions with identical geometry continue that scoped history. Related versions and constituent strikes remain separately linked under Layer 1 lineage; changing a cluster does not silently inherit a combined counter. Index 1 means the first observed episode in that scope, not necessarily the first market test of the day.

Repeated processing, restarts and snapshot refreshes must not open duplicate episodes or reset a departure gate. Keep at most one active episode and one active breakout watch for the same exact geometry/timeframe. A fresh snapshot alone cannot re-arm a resolved zone. Record multi-version lineage conflicts as observations rather than cloning one active interaction across successor versions.

## 6. Approach, contact and spatial ownership

For frozen zone `[A, B)`, width `W = B - A`, determine an origin from a prior completed close strictly outside the band: above B gives `FROM_ABOVE`; below A gives `FROM_BELOW`. A close exactly at a boundary alone does not establish a new exterior approach; retain an already established origin or wait for exterior evidence. Lock the approach at episode initiation.

An actual upper-boundary contact requires `L <= B <= H`; a lower-boundary contact requires `L <= A <= H`. This includes exact touches and prevents a candle entirely gapped beyond a band from being mistaken for a traded touch. A gap across the entire band is recorded as `GAP_ACROSS_ZONE`, not as a rejection or ordinary touched-zone breakout setup.

Point membership remains half-open as specified by Layer 1. For an exact shared-boundary contact, a known approach from above assigns the exterior boundary test to the lower adjacent zone; a known approach from below assigns it to the upper adjacent zone. Do not also count a test of the transit zone solely because that exact point belongs to it.

Bars whose ranges intersect multiple zones carry `MULTI_ZONE_SPAN`. Each prospective reaction still needs its own known approach and full qualifying evidence; range overlap alone does not create multiple tests or rejection labels. If competing ownership cannot be established without guessing the intrabar path, retain ambiguous observations and emit no candidate for that ambiguous interaction.

## 7. Testing, resolution and stall

While `TESTING`, append completed bars to the same episode. The locked approach survives inside closes and multi-candle consolidation. The eventual rejection candle itself must still touch the relevant boundary; a later candle entirely outside cannot borrow a previous candle's wick.

Resolve on the first applicable completed-bar event:

1. Invalid or missing price sequence: `INVALIDATED_DATA_GAP` (or explicit malformed-input reason).
2. Full-zone traversal and recovery: `ZONE_TRAVERSAL_RECOVERY`.
3. Return to the origin boundary/side: qualifying rejection or `UNQUALIFIED_RETURN`.
4. Fresh opposite-side buffered close: `BREAKOUT`, creating a pending watch if initiation data is eligible.
5. Opposite-side close without sufficient displacement: `UNCONFIRMED_BREAK`; no pending watch.
6. Otherwise remain testing and update the consecutive inside-close counter.

For stall counting, inside means `A < close < B`; boundary recoveries resolve instead. Reset the inside streak on a non-inside close. If the consecutive inside streak exceeds M with no earlier resolution, resolve `STALL`: the 11th inside close on 1m or 6th on 5m. Record total episode bars separately from the inside streak. A stall does not establish absorption, saturation or predictive fatigue.

An unconfirmed break is an observation, not permission to promote a later drift outside the band into a fresh crossing. Subsequent setups require re-arming and a new qualifying encounter. Each episode has one terminal resolution.

## 8. Rejection geometry and immediate confirmation

Let `R = H - L`. Valid rejection geometry requires `R >= 0.50`; zero and smaller ranges remain observations but cannot qualify.

| Requirement | Support rejection | Resistance rejection |
|---|---|---|
| Locked approach | FROM_ABOVE | FROM_BELOW |
| Boundary contact | `L <= B <= H` | `L <= A <= H` |
| Recovery | `C >= B` | `C <= A` |
| Directional wick | `(min(O,C) - L) / R >= 0.40` | `(H - max(O,C)) / R >= 0.40` |
| Directional close location | `(C - L) / R >= 2/3` | `(H - C) / R >= 2/3` |
| Ordinary-rejection depth | `L >= A` | `H <= B` |

Every condition must pass. Emit pattern evidence when this completed candle becomes available; emit a candidate at the same evaluation only if eligibility also passes. There is no next-candle extreme-clearance requirement in the baseline.

A FROM_ABOVE candle with `L < A` and `C >= B`, or a FROM_BELOW candle with `H > B` and `C <= A`, is `ZONE_TRAVERSAL_RECOVERY`. It cannot also produce an ordinary rejection candidate. This describes geometry, not a proven cause such as a liquidity vacuum. Equality at the far boundary is not excessive traversal under this baseline.

For the earlier example O=7724.80, H=7726.50, L=7721.00, C=7721.16, the upper wick fraction is `1.70 / 5.50 = 0.309...`, below 0.40. It is an `UNQUALIFIED_RETURN`, not a PUT rejection candidate.

## 9. Re-arming

After resolution, count departure closes starting with the following completed candle. A qualifying above close is `C >= B + D`; a below close is `C <= A - D`. Use the episode's frozen D.

Require N consecutive qualifying closes on the same side. A nonqualifying close resets the streak. Switching sides breaks the previous streak; the current qualifying close may begin a new streak of one on its own side. This never allows alternating sides to satisfy N. Missing bars reset the streak and mark history incomplete.

After N closes, the zone is READY for a touch on a subsequent candle. The final departure candle cannot also open another episode. An active breakout watch remains responsible for its retest; its retest is not duplicated as an independent rejection candidate or new exact-zone test. Once the watch ends, re-arming resumes from that terminal event to avoid stale departure credit.

## 10. Breakout initiation

A watch requires a touched-zone episode, locked origin, valid current initiation data and a fresh buffered crossing. Let `Cprev` be the immediately preceding, contiguous candle's close:

| Condition | Upward break | Downward break |
|---|---|---|
| Episode origin | FROM_BELOW | FROM_ABOVE |
| Prior close not already beyond boundary | `Cprev <= B` | `Cprev >= A` |
| Buffered breakout close | `C >= B + E` | `C <= A - E` |

Equality at the buffer passes. A close already beyond B/A without clearance on the prior candle prevents a new watch from mere later drift. Record the breakout bar and frozen E. A breakout alone produces no CALL/PUT candidate under this strategy.

The breakout candle cannot be its own retest. Define `bars_elapsed = 1` for the immediately following expected candle; count scheduled intervals, not just received rows. A gap is not compressed away.

## 11. Retest watch and symmetric hold rules

The inclusive qualifying window is bars 1–15 on 1m and 1–6 on 5m. The next candle may qualify; no intervening drift candle is required. Retain exact `bars_elapsed` and optional evaluation bins 1, 2, 3–5 and 6+.

Inspect every subsequent completed candle for invalidation before testing for a candidate. For the frozen zone:

| Rule | Upward watch | Downward watch |
|---|---|---|
| Failed close | `C < B` | `C > A` |
| Excessive penetration | `L < B - 0.50*W` | `H > A + 0.50*W` |
| Retest proximity | `L <= B + 1.0` | `H >= A - 1.0` |
| Acceptable penetration | `L >= B - 0.50*W` | `H <= A + 0.50*W` |
| Confirming hold close | `C >= B` | `C <= A` |

Both proximity and penetration conditions must pass, and the candle must close holding the breakout side. Exact 50% penetration and exact boundary hold are allowed. Retests do not inherit the rejection wick/range thresholds: their confirmation is the specified boundary hold after a separate break. Record wick, body and close fractions as evidence, but do not invent another veto.

A missing expected candle terminates the watch as `INVALIDATED_DATA_GAP`. A failed close terminates it as `FAILED_BREAKOUT_TRAP`; this is a price-rule label, not proof of manipulation. Excessive penetration terminates it as `INVALIDATED_EXCESSIVE_PENETRATION` even if the close recovers. Expire before accepting a retest with elapsed bars greater than the configured maximum.

Record all applicable terminal evidence with deterministic primary precedence: price-data gap/invalidity, expiry, excessive penetration, failed close, then qualifying retest. Secondary reasons remain visible. A retest on the final permitted bar can qualify if no higher-priority condition applies.

At the first qualifying retest, record the pattern and its eligibility result and consume the watch. Emit at most one eligible candidate. Retain the terminal watch and events durably; remove it only from the active in-memory set, never delete its historical record. A retest blocked by data or session policy is consumed as blocked and is not released later. A later opportunity requires a newly formed setup and confirmation.

## 12. Pattern, eligibility and session boundaries

Keep three separate records: detected pattern, candidate-eligibility result and later outcome evaluation. Eligible does not mean quality-approved, filled or ready for unrestricted execution.

Pattern observations may finish during a GEX-data failure only when an existing frozen episode/watch still has valid required price evidence and frozen parameters. This does not permit new setups from unavailable zones or evaluation across missing price bars. Raw observations remain distinct from verified patterns.

Eligibility requires valid Layer 1 current health, a legitimate active setup, complete required price evidence, frozen ATR, causal availability, and the active session phase. A confirmation candle must start at or after 10:00 ET and end before the session close to produce an eligible candidate. Thus the earliest completed confirmation is 10:01 on 1m or 10:05 on 5m, subject to all other inputs. Exact session-close bars remain observations only.

A pre-10:00 episode/watch may provide history and remain active within its normal lifetime, but cannot be promoted automatically at 10:00. It needs a new qualifying confirmation candle beginning at or after 10:00. A pre-10:00 completed pattern is never re-emitted as fresh.

At the session close, terminate unresolved episodes/watches as `SESSION_ENDED` and retain history. Paper entry cutoffs, paper exits, position limits and fills belong to the separate execution design. Historical startup replay reconstructs observations and state, not fresh actionable alerts from already elapsed confirmations.

GEX-data failure blocks eligibility without erasing ongoing price observations. Price-bar gaps invalidate sequences because their path is unknown. After a gap, re-establish continuous approach/departure evidence; late repair does not retrospectively create a live candidate.

## 13. Immutable evidence and persistence

Store patterns even when eligibility fails, with explicit blocker reasons. Stable IDs must include session, timeframe, configuration, zone/setup identities and event identity; time and direction alone are not unique enough. Atomic, idempotent event writes prevent duplicate episodes, counts and candidates across replay or restart.

Minimum decision-time evidence includes:

- Pattern/candidate identity and direction; episode/watch identity; originating zone version and level-lineage references.
- Bar start/end, bar availability, evaluation timestamp, configuration/model version and session phase.
- Frozen zone bounds/member strikes, initial dominance/composition and original input completeness.
- Frozen ATR, window/reference/availability timestamps, D/E thresholds and parameter values.
- Locked approach, exact-zone test index/scope, observed-history completeness, episode bars and inside-close streak.
- OHLC, range, directional wick and close fractions, penetration, traversal/span flags and bars since breakout.
- Current health snapshot identity, `snapshot_asof_ts`, `available_ts`, coverage, TTL age and blockers; retain per-contract provenance through that snapshot.
- Eligibility outcome and terminal state; observed shape and eligibility must remain distinguishable.

No future confirmation or forward return is a field that decision evaluation can query. Keep raw ratios even on rejected cases; an invalid fraction is unavailable, not silently zero.

## 14. Detached outcome evaluation and causal comparison

Outcome records link to a pattern/candidate without mutating the original evidence. Record outcomes for blocked observations as well when inputs permit, preserving their blocked status. This allows comparisons without selecting only already eligible patterns.

For a rejection at candle t:

| Outcome | CALL | PUT |
|---|---|---|
| `next_bar_extreme_exceeded` | `H[t+1] > H[t]` | `L[t+1] < L[t]` |
| `next_bar_close_confirmed` | `C[t+1] > H[t]` | `C[t+1] < L[t]` |

Use NULL with an explicit reason if the expected next candle is unavailable; do not substitute a later bar. Outcomes become available only after their supporting data. Equality does not confirm these strict tests.

Correct the proposed delayed-price shortcut: the open of t+1 occurs before t+1 confirmation and cannot be the entry price of a policy that waits for that confirmation. Baseline and delayed comparisons must use separate decision timestamps. The rejection close at t and a confirming close at t+1 can be labeled descriptive SPX references; any simulated executable entry must use causal post-decision data under a separate fill policy. The t+1 open may be retained as a separate observation only, not delay slippage or a confirmed-entry fill. OHLC extreme crossing alone does not establish crossing time or fill price.

Measure forward MFE/MAE over the next 3, 5 and 15 expected bars, excluding the signal candle's earlier path. Record the horizon unit, reference price/time and complete-window status. With direction d=+1 for CALL and -1 for PUT and descriptive reference C[t], favorable excursion is the maximum nonnegative directional move from that reference; adverse excursion is the maximum nonnegative move against it. These are underlying SPX-point observations, not realized option P&L. Missing windows or session-end truncation stay explicitly incomplete; they are not compressed, zero-filled or extended into the next session.

## 15. Acceptance cases and remaining scope

Required implementation checks:

1. One multi-candle encounter creates one episode/test index; re-arming increments neither.
2. Index assignment survives late-session termination and idempotent replay; related-zone counts do not silently merge.
3. Upper/lower boundary equality is handled symmetrically; a whole-zone gap is not a traded touch.
4. Range exactly 0.50, wick exactly 0.40 and close placement exactly 2/3 pass; values below fail.
5. The 30.9% upper-wick example produces no rejection candidate.
6. Symmetric full-zone recoveries are excluded from ordinary rejections.
7. ATR uses the recorded 130-bar seed/window and prior close; missing inputs do not activate point-floor fallback.
8. A resolution candle does not count toward re-arming; side changes cannot complete a mixed-side streak.
9. Stalls resolve on the 11th/6th consecutive inside close, not on the 10th/5th.
10. A first-next-candle retest can qualify; the breakout candle cannot retest itself.
11. Unbuffered closes and already-cleared prior closes cannot create later drift-based breakout watches.
12. Retests on bars 15/6 can qualify; bars 16/7 cannot. Missing intervals invalidate rather than extend the watch.
13. Exactly 50% penetration is allowed; deeper wicks invalidate despite recovery. Failed-close rules mirror correctly.
14. Each watch has at most one candidate; blocked triggers are not queued; consumed records survive restart.
15. Morning observations persist, but candidates require a fresh confirmation candle starting at or after 10:00.
16. Current health uses a current snapshot, while geometry/ATR remain frozen; unavailable historical inputs cannot be borrowed from the future.
17. The 1m and 5m paths remain independent; shared-boundary observations do not generate duplicate ownership events.
18. Next-bar confirmations remain detached; delayed comparisons cannot enter at the earlier t+1 open or infer option fills.

Remaining layer documents will define regime/environment evidence, any quality-score formula, multi-timeframe comparisons, and paper execution. Layer 2 assigns no success probabilities, fatigue penalties or unvalidated execution tier. Statistical claims about delay costs, retest frequency or setup performance require separate empirical evaluation.
