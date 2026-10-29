# Layer 3 — Environmental Context

Status: **Frozen design, version 1.0 — 2026-10-29.**

This document follows [Layer 1 — GEX Zone Discovery and History](layer_1_zone_discovery.md) and [Layer 2 — Price Reactions and Setup Evaluation](layer_2_price_reactions.md). It supersedes the original regime bonuses, volatility vetoes and participation assumptions in `ARCHITECTURE.md`.

Frozen means the first research specification is agreed. It does not mean implemented, predictive, statistically validated or authorized for live execution. Changes require an explicit version revision. Existing `spx_orb` behavior remains separate.

## 1. Baseline policy and responsibility

Layer 3 observes the environment around a Layer 2 candidate. **Missing, incomplete, stale or conflicting Layer 3 context must never block, discard, delay or change an eligible Layer 2 candidate.** Candidate emission cannot wait for an optional network request or calculation to succeed.

The layer records:

1. Broader modeled GEX and the surrounding exposure excluding the candidate zone.
2. Completed VIX/VIX1D levels, 15-minute changes and normalized slopes.
3. SPY participation through rolling and time-of-day relative volume.

Quality thresholds here govern whether a measurement is usable, not whether a trade is permitted. There are no score bonuses, fatigue penalties, position decisions, directional vetoes or execution instructions. Labels such as supportive or conflicting are research descriptions, not demonstrated success probabilities.

Layer 1's local data-health requirements and Layer 2's setup requirements remain binding. A failure confined to Layer 3's broader universe must not be misreported as a failure of otherwise valid local inputs. Conversely, Layer 3 cannot override a genuine Layer 1/2 blocker.

## 2. Candidate clocks and immutable observations

Each record references the original candidate ID, CALL/PUT side, timeframe, confirmation-bar interval and actual decision timestamp. Use timezone-aware timestamps, an ET session calendar and durable source/configuration identities.

The original context may use only inputs completed and available by the candidate decision. Volatility and volume measurements align to the candidate's confirmation-bar end, not a later bar that happens to exist when background processing runs.

If context construction completes later, distinguish computation/publication time from the candidate's input-availability cutoff. The original context must still use the recorded as-of input set. Inputs that actually arrive after the decision belong in an append-only `enrichment_audit` with their own availability times. They do not replace unavailable original fields or change an earlier candidate.

Persist unavailable statuses promptly and retain independent metrics that are available. A missing VIX feed does not erase valid VIX1D or SPY evidence. Replay respects source availability, historical gaps and revisions. Retrospective reconstructions without availability evidence must be labeled accordingly, not represented as live-known inputs.

## 3. GEX universe and synchronization

Use the same session's 0DTE, P.M.-settled SPXW expiration for the local and broad models in this version. Record the exact root, expiry and model/configuration; never blend other expirations silently. This selects the explicit research universe within Layer 1's configurable universe contract.

The inclusive broad strike corridor is `[snapshot_spot - 150.0, snapshot_spot + 150.0]`. Persist spot, its source/availability, the corridor bounds and valuation cutoff. The half-width is a configurable research default, not a verified claim to capture 95% or any other proportion of total market gamma.

The broad corridor contains the local corridor; the measurements are not statistically independent. Candidate-zone exclusion measures the remaining modeled exposure without claiming to eliminate all correlation.

Use the latest valid broad snapshot available by the candidate decision, respecting Layer 1's time conventions: quote age at snapshot valuation <=120 seconds and snapshot age at decision <=360 seconds. Record actual snapshot age. A new collection requested at the decision but available afterward is late enrichment, not original evidence. A newer known broad ingestion failure cannot be silently bypassed by selecting an older healthy record.

For synchronized local-versus-broad comparisons, use one broad snapshot's contract measurements and the frozen candidate zone's member-strike set. Do not subtract frozen episode-time dollar values from a later broad total. Retain original frozen composition and synchronized composition in distinct fields.

## 4. Independent baseline and broad quality

Follow Layer 1's independent reported-OI baseline, contract validation, zero-bid policy and provenance rules. The provider/file must be verified before implementation acceptance; this specification does not assume a particular official OCC download is available. OI must not be reconstructed solely from whatever quote contracts were returned.

```text
baseline_oi = sum(reported positive OI of baseline contracts in broad corridor)
usable_oi = sum(baseline OI of contracts meeting the valuation/input rules)
coverage_ratio = usable_oi / baseline_oi
```

The broad observation threshold is **85% inclusive**. Layer 1's local threshold remains 90%. Coverage is OI-weighted data coverage, not the fraction of gamma sensitivity observed; an omitted minority may still affect the sign or magnitude.

When broad coverage is below 85%, the baseline is unavailable, required valuation inputs fail or the snapshot is stale, set broad status `UNAVAILABLE`, retain specific reasons and sample metadata, and publish null environmental GEX metrics. Raw partial inputs may remain in ingestion/audit storage, separate from the usable environmental metrics.

Verified zero baseline OI and zero usable modeled gross exposure have distinct reasons, matching Layer 1. Undefined ratios are null. An unavailable measure never becomes zero, neutral, balanced or congruent. The Layer 2 candidate remains unchanged in every case.

## 5. Broad and ex-zone mathematics

All sensitivity values use the same signed model and units as Layer 1: dollars per 1% underlying move, assumed positive call signs and negative put signs. For usable contracts aggregated at strike K:

```text
gross[K] = abs(call_gex[K]) + abs(put_gex[K])
net[K] = call_gex[K] + put_gex[K]
G = sum(gross[K] in broad corridor)
N = sum(net[K] in broad corridor)
broad_balance = N / G
```

`net_gex_dollars` is signed; the earlier field name `absolute_net_gex` meant a dollar-valued net rather than a normalized ratio, not `abs(N)`. Use the unambiguous signed name. With G > 0, broad balance lies in inclusive `[-1, +1]`.

Let C be the frozen candidate-zone member strikes. Using the same broad snapshot:

```text
G_C = sum(gross[K] for K in C)
N_C = sum(net[K] for K in C)
candidate_zone_share = G_C / G
synchronized_zone_bias = N_C / G_C
G_ex = G - G_C
N_ex = N - N_C
ex_zone_balance = N_ex / G_ex
```

Publish frozen setup bias separately. Exclusion operates on member strikes, not the padded price band. Keep original member identities even if the latest clustering would assign different members.

If frozen members fall outside the current broad corridor, mark candidate share, synchronized bias and ex-zone comparison unavailable with `ZONE_OUTSIDE_BROAD_CORRIDOR`; do not silently clip the zone. Broad totals may remain valid. If candidate members have unusable positive-OI contracts, retain missing-member/side metadata and mark the full-zone comparison unavailable rather than pretending an omitted member had zero exposure. Broad totals still follow their own 85% policy.

Store candidate-zone and ex-zone OI coverage separately so broad acceptance cannot conceal where exclusions occurred. Ex-zone values use the broad snapshot's usable-contract set and retain its omission evidence; there is no additional ex-zone entry gate. If G_C or G_ex is zero, only ratios with that denominator become unavailable; do not manufacture neutral bias. A genuine measured N=0 with positive G is a valid zero net balance.

Example in consistent sensitivity units: G=100, N=-20, G_C=20 and N_C=10 produce broad balance -0.20, zone share 0.20, synchronized local bias +0.50 and ex-zone balance -0.375. This describes opposite modeled composition signs, not predicted price direction.

Any display comparison must name whether it uses frozen or synchronized local bias and whether the comparator is broad or ex-zone balance. The core evidence is the numeric pair. No undefined label such as heavily negative or regime conflict is added as an entry rule.

## 6. Volatility source and alignment

VIX measures expected 30-day volatility derived from SPX option prices. VIX1D estimates expected one-day volatility using near- and next-term SPX options with intraday weighting. Neither directly observes dealer hedging transactions or guarantees a directional SPX response.

Measure VIX and VIX1D independently using completed bars aligned to the candidate's 1m or 5m interval, with five-minute buckets anchored to 09:30 ET. Preserve provider bar conventions. Reject malformed/placeholder bars rather than treating zeros as measured volatility; do not forward-fill missing intervals.

The reference close must end at the candidate confirmation-bar end and be available by its decision. If it is missing or late, the corresponding current measurement is unavailable; do not quietly substitute an older close under the same field. Each metric retains its own status and source times.

## 7. Delta and normalized slope

For each volatility index at confirmation-bar end t:

```text
delta_15m_points = close_at(t) - close_at(t - 15 minutes)
```

Both endpoints must be exact completed-bar timestamps within the same RTH session. Do not bridge the overnight boundary or compress missing intervals into a 15-minute window. The underlying window must be complete and valid. This is 15 intervals on 1m and 3 intervals on 5m. Changes are index points, not percent changes.

Fit ordinary least squares to the last five consecutive completed closes ending at t. With bar positions x=0,1,2,3,4:

```text
slope_per_bar = sum((x - mean(x)) * (close - mean(close))) / sum((x - mean(x))**2)
slope_points_per_minute = slope_per_bar / bar_interval_minutes
```

Store the five reference timestamps, raw slope, normalized slope and elapsed duration between the first and last closes. This duration is 4 minutes for 1m and 20 minutes for 5m. Normalizing units does not make these windows equal horizons. Missing any required slope observation makes slope unavailable, independently of a valid delta or current close.

## 8. Directional consistency labels

The baseline classification uses **VIX1D** delta and slope together with the candidate side. VIX metrics remain independent raw context; they do not become a substitute when VIX1D is missing and do not veto its classification.

| Side | VIX1D delta, index points | VIX1D slope, points/minute | Label |
|---|---|---|---|
| CALL | <= -0.15 | < 0 | `DIRECTIONALLY_SUPPORTIVE` |
| CALL | >= +0.25 | > 0 | `DIRECTIONALLY_CONFLICTING` |
| PUT | >= +0.25 | > 0 | `DIRECTIONALLY_SUPPORTIVE` |
| PUT | <= -0.15 | < 0 | `DIRECTIONALLY_CONFLICTING` |
| Either | Any other complete valid combination | Including zero or disagreeing slope | `INCONCLUSIVE` |
| Either | Required input missing, invalid or late | Any | `UNAVAILABLE` |

Equality at the delta boundaries passes their respective comparisons; slope comparisons remain strict. Large deltas with contrary slopes are inconclusive rather than unclassified. Preserve all underlying measurements. The asymmetric thresholds are configurable research hypotheses, with no execution consequence. The candidate's direction does not establish the observed SPX path during the volatility window.

## 9. SPY volume input contract

Use SPY consolidated trading volume as a participation proxy, aligned to the candidate's interval. It measures trading activity, not signed capital flow or actual SPX volume. Verify provider entitlements, units, session filters, consolidation and correction conventions before implementation; do not label a partial venue feed consolidated.

The current SPY bar and reference bars must be complete, nonnegative, valid under the provider contract and available by the decision. Genuine zero volume is distinct from missing/placeholder data. Five-minute volume is the sum of all five constituent minutes using a consistent source definition. Never mix share volume, trade counts or incompatible provider filters.

Persist provider/convention versions and supporting bar identities. Late corrections are separate enrichment, not silent replacements of original context. Current and historical denominators must use comparable measurement conventions.

## 10. Rolling RVOL

```text
rolling_mean_20 = sum(volume[t-k] for k in 1..20) / 20
rvol_rolling_20 = volume[t] / rolling_mean_20
```

Use exactly the preceding 20 scheduled, complete bars in the current RTH session, excluding the current numerator bar. Missing bars are not skipped or replaced with older bars. Never pull prior-session volume into this rolling denominator.

The first eligible numerator interval is the 21st session interval:

| Timeframe | First numerator interval | Earliest completed-bar availability, before delivery delay |
|---|---|---|
| 1m | 09:50–09:51 ET | 09:51 ET |
| 5m | 11:10–11:15 ET | 11:15 ET |

Thus the 09:50/11:10 times identify numerator starts, not completed RVOL observations. For a 5m candidate whose bar ends at 10:20, there are nine preceding session bars, not ten eligible denominator samples.

Store valid `sample_count`, expected count 20, available scheduled intervals and failure reasons. Before sufficient history, use `UNAVAILABLE_INSUFFICIENT_HISTORY`; missing required bars after warm-up have an explicit missing-input reason. If the denominator is zero, use `UNAVAILABLE_ZERO_DENOMINATOR`. Zero numerator with positive denominator is a valid RVOL of zero.

## 11. Time-of-day RVOL

```text
rvol_tod = current_interval_volume / median(valid_matching_historical_volumes)
```

Select the preceding **ten trading sessions** from the calendar, excluding today. Within that fixed session list, match the same ET start/end interval and candidate timeframe. Require at least eight valid matches; use the median of the valid 8, 9 or 10 values. For an even count, median is the average of the two middle sorted values.

This resolves the earlier phrase full sessions by the explicit interval policy: an early-close session contributes when the entire matching interval existed, but cannot contribute to an interval after its close. Do not discard its valid morning interval, treat an absent afternoon interval as zero, or reach back to an eleventh session to hide the missing match. Calendar holidays do not count as sessions. Document calendar/timezone handling across daylight-saving transitions.

Store the selected ten session dates, valid match dates/count, excluded dates/reasons, median and minimum required count 8. Fewer than eight valid matches produces `UNAVAILABLE_INSUFFICIENT_DAYS`. A zero median produces `UNAVAILABLE_ZERO_DENOMINATOR`. No winsorization or additional outlier gate is introduced by this baseline; a zero-denominator check is not an outlier filter.

Rolling and time-of-day RVOL have separate statuses. One being unavailable must not suppress the other or the candidate. Both are raw ratios, with no binary participation gate or bonus score.

## 12. Environmental record and status contract

Persist one immutable original environmental record per candidate/configuration identity, with explicit partial availability. The following is the minimum logical payload; physical database layout remains an implementation concern:

| Group | Required evidence |
|---|---|
| Identity/time | Candidate ID/side/timeframe, session, confirmation interval, decision cutoff, context creation/publication time, configuration version |
| Frozen local context | Original zone version, member strikes, bounds, frozen bias/dominance, original snapshot identity |
| Broad provenance | Snapshot identity/as-of/availability, age, spot, corridor, exact expiration, model, OI baseline identity, usable/baseline OI and exclusion metadata |
| Broad metrics | Signed net dollars, modeled gross dollars, balance and independent statuses/reasons |
| Synchronized comparison | Same-snapshot zone gross/net/bias/share, ex-zone gross/net/balance, subgroup coverage, frozen-vs-current distinction |
| VIX and VIX1D | Independent closes/deltas/slopes with bar identities, availability, sample counts, units and elapsed windows |
| Classification | Candidate side, VIX1D directional label, threshold/configuration version and input status |
| Participation | SPY provider convention, interval volume, rolling denominator/count/status, TOD dates/count/median/status |

Use finite numeric values only when defined. Unavailable numeric fields are null with specific reasons; never use zero or NaN as a missing-data substitute. Keep all applicable reasons rather than hiding a feed error behind an unrelated warm-up status. A top-level partial-context summary must not erase valid per-field observations.

Layer 3 failures do not mutate the Layer 2 candidate's eligibility or terminal setup state. Independent asynchronous collection may populate caches, but original records are built against the fixed cutoff. Failures and recoveries remain observable without becoming trading rules.

## 13. Persistence, replay and evaluation boundaries

Original context writes are idempotent under candidate/configuration identity. Store source references sufficiently to reconstruct all arithmetic and as-of choices. Restarts must not create new candidates, overwrite original missingness or attach later measurements as if they were originally available.

`enrichment_audit` retains late inputs, revised values and actual arrival times separately. The original decision record and its missingness remain queryable. Forward returns and MFE/MAE stay in the detached outcome storage defined in Layer 2; neither original context construction nor future live evaluators may read them as inputs.

Research must retain candidates with incomplete context and report sample availability by time of day/timeframe. Do not drop unavailable-context candidates from the baseline performance population by accident. Any later study restricted to complete observations must disclose that selection and compare it to the full baseline.

## 14. Acceptance examples

1. An eligible 10:20 5m candidate survives unavailable rolling RVOL, absent VIX1D and incomplete broad GEX; each factor carries its own reason.
2. Broad coverage of exactly 85% passes; below 85% nulls broad environmental metrics without changing local 90% requirements or candidate eligibility.
3. Missing independent OI baseline is unavailable, never a zero-exposure observation. Zero bid does not imply zero modeled gamma.
4. Synchronized zone subtraction uses the broad snapshot, not frozen episode-time dollar values. Out-of-corridor members are explicit.
5. G=100, N=-20, G_C=20, N_C=10 produces balance -0.20, share 0.20, synchronized bias +0.50 and ex-zone balance -0.375.
6. Zero gross denominators yield null ratios; zero net with positive gross yields valid zero. Unavailable inputs never become a congruent label.
7. A late snapshot or late VIX/SPY bar only appears in enrichment audit. Later computation cannot move the original input cutoff.
8. Exact 15-minute endpoints and five consecutive slope observations are required; 1m/5m slopes use 4/20-minute elapsed spans and point-per-minute units.
9. CALL delta -0.15 with negative slope is supportive; PUT delta +0.25 with positive slope is supportive. Zero or contrary slope is inconclusive; missing required input is unavailable.
10. Missing VIX does not erase valid VIX1D classification. Classification never vetoes a candidate.
11. Rolling RVOL first completes at 09:51 on 1m and 11:15 on 5m; current volume is excluded from its 20-bar denominator.
12. A zero numerator can yield valid zero RVOL; a zero denominator cannot. Missing bars are not zero-volume bars.
13. TOD uses the fixed prior-ten-session list: eight valid matches pass, seven fail, and no eleventh-session backfill occurs.
14. An early-close session can supply a matching morning interval but not a post-close interval. Current-day volume never enters historical TOD references.
15. Repeated processing and restart preserve original context, missingness and candidate identity; later outcomes cannot alter evidence.

## References and remaining scope

- [Cboe VIX Methodology](https://cdn.cboe.com/resources/vix/VIX_Methodology.pdf): expected 30-day volatility measurement.
- [Cboe VIX1D Methodology](https://cdn.cboe.com/api/global/us_indices/governance/Volatility_Index_Methodology_Cboe_1-Day_Volatility_Index.pdf): near-/next-term SPX option inputs and one-day volatility construction.

These references establish index definitions, not the predictive validity of this document's thresholds. Provider accessibility, data conventions and as-of reconstruction require implementation verification. The +/-150 corridor, 85% coverage and directional label thresholds remain research parameters.

Layer 4 multi-timeframe evidence, combined quality scoring and any execution policy remain separate design work. This layer freezes measurements and availability semantics without approving those later policies.
