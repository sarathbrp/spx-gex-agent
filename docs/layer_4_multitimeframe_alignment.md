# Layer 4 — Multi-Timeframe Alignment

Status: **Frozen design, version 1.0 — 2026-10-29.**

This specification follows [Layer 1](layer_1_zone_discovery.md), [Layer 2](layer_2_price_reactions.md) and [Layer 3](layer_3_environmental_context.md). It supersedes the original multi-timeframe bonuses, forming-candle trap veto and early-entry proposals in `ARCHITECTURE.md`.

Frozen means agreed for the first research version, not implemented or empirically validated. Changes require an explicit revision. Existing `spx_orb` behavior remains separate.

## 1. Baseline policy

Layer 4 is **observation-only**. Preserve every eligible Layer 2 candidate regardless of agreement, conflict, pending confirmation or unavailable counterpart data. Never delay emission, alter candidate direction, cancel a candidate, add score points or infer a trade fill because of multi-timeframe evidence.

The layer compares independently emitted 1-minute and 5-minute candidates on the same known zone. The engines share underlying price data, so agreement is not independent statistical confirmation. Observed agreement does not establish profitability or guarantee that either setup remains tradable afterward.

Keep the full Layer 2 baseline population so research can compare standalone candidates with a separately modeled waiting policy. A future filtered strategy may be studied explicitly, but must not erase the baseline or disguise the selected sample as the full population.

## 2. Identity and matching scope

A counterpart must be from the other timeframe: 1m compares with 5m, and 5m compares with 1m. Compare eligible Layer 2 candidates, not forming patterns, provisional prices, raw data-blocked detections or previously consumed watches without an emitted candidate.

Matching requires:

- The same session, underlying cash SPX, modeled expiration universe and compatible strategy/configuration version.
- Identical member-strike sets, independent of list ordering.
- Matching frozen lower and upper bounds, each within an inclusive 0.01 SPX-point numeric tolerance.
- A valid causal temporal match under section 3.

Different snapshot/zone-version IDs may match if their member sets and geometry meet these conditions. Shared lineage, nearby centers or a shared seed alone cannot establish a match. Store bounds canonically and apply tolerance only for numeric representation, not to make materially different geometries equivalent. Do not use tolerance matches transitively to merge zone histories.

An isolated {7725} zone and a {7725, 7730} zone are `PARTIAL_OVERLAP_NO_MATCH` diagnostic evidence. This is a reason, not a sixth top-level comparison state. No geometry is changed to force agreement.

Matching configuration identity includes the common experiment/version while allowing its intentional 1m/5m parameter differences. Comparing alternative experiment variants requires explicit research labeling and cannot silently enter the baseline.

## 3. Two clocks and the 300-second window

For primary candidate P, retain confirmation-bar end `p_end`, actual candidate decision/emission time `p_decision` and the observed-event cursor at that decision. For counterpart Q, retain confirmation-bar end `q_end` and actual candidate availability `q_available`.

Decision-time matching requires all of:

```text
Q was observed by P's decision cutoff/event cursor
q_available <= p_decision
0 <= p_decision - q_available <= 300 seconds
0 <= p_decision - q_end <= 300 seconds
abs(p_end - q_end) <= 300 seconds
```

Both completion time and availability are required. A setup completed ten minutes ago but delivered now is not a fresh five-minute counterpart. Exactly 300 seconds is eligible; greater differences are not. The interval limit is a configurable research parameter, preserved with each record.

Candidate availability means the candidate and its required evidence were actually published to the comparison system, not merely that its source candle closed. Same-wall-clock timestamps do not justify reading an event that was processed later; use recorded event ordering to make simultaneous-close replay deterministic.

Store both `completion_delta_seconds` and `availability_age_seconds`, rather than one ambiguous delta. Preserve signed completion ordering as well as absolute age. Inputs cannot be backdated to make a match eligible.

## 4. The relevant counterpart interval

`PENDING_CLOSE` refers to the counterpart interval covering the primary confirmation instant immediately before `p_end`, not whichever candle happens to be forming when the comparator runs.

For regularly aligned buckets, the relevant interval has `start < p_end <= end`. At an exact shared boundary, select the interval ending there rather than the new interval beginning there.

| Primary confirmation | Relevant counterpart interval |
|---|---|
| 1m candle ending 10:07 | 5m interval 10:05–10:10 |
| 1m candle ending 10:10 | 5m interval 10:05–10:10 |
| 5m candle ending 10:10 | 1m interval 10:09–10:10 |

Use `p_decision` to decide whether that particular interval has completed. If its end is still in the future, it is forming. If its end has passed but its required bar or evaluation is not available, it is unavailable/pending delivery, not a forming candle. The comparator does not wait for delivery or invent a latency grace period to improve the original record.

This distinction makes `NO_MATCH` reachable at completed boundaries even while the next market candle is already forming.

## 5. Health evidence and state priority

Evaluate counterpart health within the required recent comparison/interval scope. Required missing or malformed bars, invalid timestamps, unavailable evaluator results or an explicitly reported pipeline-health violation make the comparison unavailable. Record the affected intervals and source reasons. Expected market closures are not data gaps, and an unrelated old-session problem does not automatically invalidate the current comparison.

A no-candidate result is known only when the counterpart evaluation has completed. Missing evaluation must not masquerade as a complete evaluation that found no pattern. Current evaluation availability is distinct from Layer 3 environmental availability; missing VIX/RVOL cannot make Layer 4 unavailable when the required Layer 2 evidence is present.

Resolve the original state in this order:

1. **`UNAVAILABLE`:** required counterpart data/evaluation is unavailable or invalid.
2. **`DIRECTIONAL_CONFLICT`:** at least one spatially/temporally matching eligible counterpart has the opposite CALL/PUT side.
3. **`AGREEMENT`:** at least one matching counterpart has the same side and none has the opposite side.
4. **`PENDING_CLOSE`:** no matching counterpart exists and the specific relevant counterpart interval is still forming.
5. **`NO_MATCH`:** the relevant interval and evaluation are complete and valid, but no matching counterpart candidate exists.

Completed-match evidence takes priority over a forming relevant candle when health is valid. The presence of a forming candle does not force pending status if a recent completed match already exists.

An unavailable top-level state does not erase otherwise known candidate evidence; retain it with the health blocker. The hierarchy controls the summary only, not whether a candidate is preserved.

## 6. Multiple counterpart candidates and family alignment

Store all qualifying counterpart IDs and per-pair evidence. Do not stop at the first matching row returned by storage.

If both same-side and opposite-side matches exist, the summary is `DIRECTIONAL_CONFLICT`, with `has_same_side_match` and `has_opposite_side_match` both retained. This is a deterministic descriptive priority, not a recommendation to reverse or discard the primary candidate.

When a single representative ID is needed for display, choose from the priority-winning direction group, then prefer latest confirmation-bar end, latest availability, and lexicographically smallest stable candidate ID for exact ties. Preserve the full match list regardless of the representative.

| Per-pair relationship | Family label |
|---|---|
| Same side and identical Layer 2 setup type | `HOMOGENEOUS_AGREEMENT` |
| Same side but different Layer 2 setup type | `HETEROGENEOUS_AGREEMENT` |
| Opposite side | `NONE` for agreement family; preserve both setup types |
| No counterpart | `NONE`; comparison fields unavailable |

For example, support rejection plus support rejection is homogeneous; support rejection plus upward break/retest is heterogeneous. These labels are additional evidence under AGREEMENT, not replacements for the five-state taxonomy.

If no counterpart exists, `spatial_zone_match` is null/NOT_EVALUATED, not true. The primary's zone metadata describes the target being sought, not proof of a match.

## 7. Forming-candle isolation

Provisional highs, lows, last prices and wicks may be displayed in telemetry with their own observation timestamp and a clear forming flag. They must not enter formal agreement/conflict matching, family labels, candidate eligibility, scores or execution decisions.

Known interval boundaries may establish that a relevant candle is still forming; its provisional shape cannot establish a completed setup. Formal comparison uses only completed Layer 2 candidate events.

At 10:08, a 5m wick can therefore coexist with `PENDING_CLOSE`, an existing completed-match state, or `UNAVAILABLE`, depending on the hierarchy. It never justifies a provisional false-break veto.

When a completed candle later returns inside a zone without producing a counterpart candidate, retain that completed-bar behavior separately. Absence of a matching candidate and an observed inside close are not automatically `DIRECTIONAL_CONFLICT` or a validated false-break trap.

## 8. Immutable decision-time record

`candidate_decision_mtf` records what was known at P's decision. Logical identity is `(candidate_id, comparison_config_version)`; comparison reruns under different research configurations must not overwrite one another.

Required fields include:

- Candidate ID, side, setup type, session, experiment/configuration and underlying/expiration identities.
- Primary/counterpart timeframes, primary bar interval, actual decision time, recorded event cutoff and comparator publication time.
- Frozen zone version, exact member strikes, bounds and matching tolerance/window.
- Five-state summary, health status/reasons and scoped missing intervals/evaluation evidence.
- Relevant counterpart interval, completion/availability state and latest required completed counterpart evidence.
- All matching counterpart IDs, sides, setup types, source versions, completion/availability times and both clock deltas.
- Representative counterpart, per-pair family labels, same/opposite-match flags and overlap/nonmatch reasons.

The record is immutable. Later processing may publish it using the original recorded input set; it must not expand the as-of cutoff. Known-missing original evidence remains missing even after a feed repair. Layer 4 failures cannot delay or remove the original Layer 2 event.

Forming telemetry belongs in a separate stream or separately tagged observation payload. It must not be passed as an input to the formal matching function.

## 9. Subsequent comparison events

Maintain an append-only `subsequent_mtf_events` audit. Observe candidates from **all five original states**, not only PENDING_CLOSE. An originally agreeing candidate can later encounter conflict, and an unavailable comparison can later receive evidence; limiting follow-up to pending rows would selectively discard these histories.

The standard follow-up horizon is 300 seconds after the primary decision, bounded by the session end. Subsequent counterpart candidates must still satisfy exact spatial/configuration matching and confirmation-bar separation no greater than 300 seconds. Their availability must be after the primary cutoff and no later than the follow-up deadline. At exactly equal wall-clock times, use the recorded event order to determine subsequent knowledge.

Distinguish a genuinely later confirmation from an earlier/shared-close confirmation delivered later. Store both clocks and an event-timing classification. A late old setup cannot become eligible for comparison solely because its arrival is recent. Inputs arriving beyond the horizon may be retained as late-data audit records, but do not extend the original matching window or rewrite its results.

Events include:

- `SUBSEQUENT_AGREEMENT` and `SUBSEQUENT_CONFLICT`, with counterpart candidate references and family metadata where applicable.
- `SUBSEQUENT_NO_MATCH` when the relevant counterpart interval/evaluation completes without a match.
- Counterpart data-health observations and `COMPARISON_WINDOW_EXPIRED`/`SESSION_ENDED` when applicable.

A no-match event at the relevant close does not prevent recording another comparable candidate before the horizon ends. Follow-up captures all qualifying events rather than only the first favorable result. A later inside-zone close is linked as descriptive completed-bar evidence, not invented as a candidate.

Event records contain stable event identity, primary candidate/configuration identity, event type, actual availability, counterpart completion and candidate ID where one exists, supporting bar identities and elapsed time/interval counts. Counterpart ID is nullable for no-match, health and expiry events.

Neither a subsequent agreement nor a later conflict changes the original state. The audit does not execute, cancel or re-emit a Layer 2 candidate. All original candidates and terminal records survive restart.

## 10. Reference movement and waiting-policy evaluation

The proposed raw price difference is **not execution slippage**. Define a direction-adjusted descriptive SPX reference move:

```text
d = +1 for CALL, -1 for PUT
directional_reference_move_points = d * (later_reference_price - primary_reference_price)
```

Positive means SPX moved in the primary candidate's direction before the later reference; negative means it moved against it. For a CALL, 7720 to 7724 gives +4 points. For a PUT, 7720 to 7716 also gives +4 points. Neither establishes the change in option premium or an obtainable fill.

Persist reference price definitions, source times and availability. Completed confirmation closes may be used as descriptive references under Layer 2's outcome contract. Label the field `directional_reference_move_points` rather than `delayed_entry_slippage_points`. Missing reference prices remain null.

Price-path comparisons and this derived movement belong to detached evaluation storage linked to the subsequent event, rather than the original matching inputs. Keep forward outcomes inaccessible to decision-time comparison.

A waiting-policy study must identify which eligible subsequent event triggers its later decision, evaluate that policy using only then-available evidence, and assign a causal post-decision execution reference or fill model. It cannot choose the best later confirmation retrospectively or enter at a candle open preceding the confirmation. Actual execution slippage requires an explicitly defined execution benchmark and fill evidence; it is not inferred from two SPX closes.

Elapsed bars are measured in the primary timeframe and retain the exact timestamps/elapsed seconds. Missing expected bars are not silently compressed out of delays. Cross-timeframe overlap is identified in reports; reciprocal links are not treated as independent market observations.

## 11. Worked timing cases

**Pending then later agreement:** A 1m CALL becomes available at 10:07:01, with its confirmation ending 10:07. With healthy data, no recent counterpart and the relevant 5m interval 10:05–10:10 still forming, its original state is PENDING_CLOSE. A matching 5m CALL becomes available at 10:10:02. Append subsequent agreement; leave the original row unchanged. A reference comparison uses declared prices and does not invent a 10:07 fill for the waiting policy.

**Existing match while another candle forms:** A matching 5m CALL ending 10:05 was available at 10:05:02. For a 1m CALL at 10:07:01, that candidate is timely and known. With healthy data and no opposite match, original state is AGREEMENT even though 10:05–10:10 is forming.

**Shared close but later delivery:** A 1m candidate is available at 10:10:01, but the relevant 5m evaluation is not available until 10:10:02. The relevant interval has already ended; the first candidate records UNAVAILABLE with a delivery/evaluation reason, not PENDING_CLOSE for 10:10–10:15. Later candidate evidence creates a subsequent event. Its zero completion-time separation is distinct from its nonzero knowledge delay.

**Complete evaluation with no match:** At the same shared boundary, if the relevant 5m bar and its no-candidate evaluation are already available, and no recent comparable candidate exists, original state is NO_MATCH. The newly forming 10:10–10:15 candle does not alter that result.

**Old signal delivered now:** A 5m setup ending 09:55 arrives at 10:07. It cannot match a primary signal ending 10:07 merely because availability is recent; the completion window fails. Record late data separately.

## 12. Persistence, reconstruction and acceptance

Persist the original comparison and subsequent events idempotently. Pair identity includes primary ID, counterpart ID where present, comparison configuration and event type; use source event identities for noncandidate events. Repeated ingestion cannot duplicate audit events. Reverse-direction links may be useful for each candidate's view, but reports must distinguish candidate-level counts from unique counterpart-pair counts.

Replay restores state from recorded candidate/evaluation availability and event ordering. Repaired bars or late counterpart events create new audit evidence; they cannot erase original missingness or rewrite a decision-time state. Restart must retain follow-up deadlines and already recorded terminal events without extending the horizon.

Required implementation checks:

1. Every eligible Layer 2 candidate survives all five comparison states and all Layer 4 errors.
2. Same underlying/session/universe/configuration and exact member sets are required; partial overlap cannot establish agreement.
3. Each bound at 0.01-point difference passes tolerance; larger differences fail without transitive geometry merging.
4. Both availability and completion windows are enforced; exact 300-second boundaries pass and greater ages fail.
5. Identical wall-clock timestamps obey recorded event ordering; no counterpart processed later leaks into an earlier decision.
6. Missing required data wins summary priority but does not delete known evidence.
7. When same- and opposite-side matches coexist, conflict wins the summary, all matches remain stored, and the representative tie-break is deterministic.
8. Homogeneous and heterogeneous agreement are distinguished; absence of a counterpart never sets spatial match to true.
9. The relevant interval ending at a shared boundary is selected instead of the newly forming interval; complete no-candidate evaluation produces NO_MATCH.
10. Completed recent agreement outranks pending status; forming wick shape never creates agreement/conflict or a veto.
11. Later events follow candidates from every original state, with the original rows immutable.
12. Subsequent same-close delivery, genuinely later confirmation, beyond-window arrivals and no-match completion remain distinguishable.
13. CALL upward and PUT downward reference moves have the same positive direction-adjusted sign; neither is labeled an execution fill or slippage.
14. Waiting policies cannot use pre-confirmation opens, later-selected best events or future outcomes in decision matching.
15. Restart/replay preserve candidate IDs, comparison cutoffs, audit deduplication and deadlines without re-emission.

## Remaining design work

Layers 1–4 now specify zone discovery, reaction candidates, environmental context and multi-timeframe observations. Their thresholds remain research parameters. A combined quality score, calibration method, execution/portfolio policy, application layout and implementation plan require separate agreement. The original 15-point alignment bonus and false-break auto-veto are not authorized by this contract.
