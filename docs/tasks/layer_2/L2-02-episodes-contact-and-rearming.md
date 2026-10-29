# L2-02 — Interaction episodes, contact ownership and re-arming

**Status:** NOT_STARTED  
**Group:** layer_2  
**Task type:** Implementation  
**Owner:** Unassigned  
**Created:** 2026-10-29  
**Specification baseline:** Frozen v1.0

[Master checklist](../../TASKS.md) · [Requirements traceability](../TRACEABILITY.md)

## Objective

Count one causal test per encounter and re-arm deterministically without boundary noise.

## Completion checklist

- [ ] Implementation or deliverable completed
- [ ] Tests / verification checks passed
- [ ] Acceptance criteria verified
- [ ] Reviewed and evidence recorded
- [ ] Complete — master index updated

Every box starts unchecked. Creating this plan is not implementation or testing. Update this task and its master-index entry together; do not mark Complete until required checks and acceptance evidence are recorded. A verification task completes its stated evidence deliverable rather than claiming application code was implemented.

## Authoritative requirements

- [Layer 2 specification](../../layer_2_price_reactions.md) — sections 5, 6, 7, 9.
- [Layer 1 specification](../../layer_1_zone_discovery.md) — sections 9, 10.

The linked frozen specifications take precedence over this task summary. Do not silently introduce new strategy rules. Record any material unresolved input or implementation decision below before dependent work proceeds.

## Dependencies

- [ ] [L1-04 — Snapshot eligibility, phases and zone handoff](../layer_1/L1-04-snapshot-eligibility-and-session-observation.md) completed, or an explicitly documented interface prerequisite is available.
- [ ] [L2-01 — Causal ATR warm-up and frozen buffers](../layer_2/L2-01-atr-warmup-and-frozen-buffers.md) completed, or an explicitly documented interface prerequisite is available.
- [ ] [P05 — Atomic transitions, projections and checkpoints](../storage/P05-atomic-transitions-and-checkpoints.md) completed, or an explicitly documented interface prerequisite is available.

Dependencies describe prerequisites, not permission to change another task's scope. Tests may use agreed fixtures/interfaces before full integration; acceptance still requires the actual dependencies and relevant integration checks.

## Implementation / deliverable checklist

- [ ] Implement READY/TESTING/RESOLVED/RE_ARMING with one test index at episode initiation, scoped by session/timeframe/config/geometry.
- [ ] Lock approach/zone/ATR, accumulate inside-close streaks and total episode bars, and record terminal reasons.
- [ ] Distinguish boundary contact, gap-across-zone, shared-border ownership and ambiguous multi-zone spans.
- [ ] Start departure counting after resolution; require same-side qualifying closes, preserve watch ownership of retests and reset on gaps.

## Required tests and verification

- [ ] Verify multi-candle encounter counts once, late-session initiation counts immediately and refresh does not re-arm.
- [ ] Test exact touches, wholly gapped bars, ambiguous spans and both approach directions.
- [ ] Verify stall on 11th/6th inside close, re-arm after 3/1 subsequent closes and no mixed-side streak or same-candle restart.

Run focused tests for this task and relevant integration/regression checks. Record actual commands, versions and results; an unexecuted test plan is not a passing result. Follow applicable repository testing and change-process instructions during implementation.

## Assigned frozen acceptance cases

- [ ] **L1-AC11** — Exact boundary ownership is deterministic, and multi-zone spans do not invent an intrabar path. ([source](../../layer_1_zone_discovery.md)).
- [ ] **L2-AC01** — One multi-candle encounter creates one episode/test index; re-arming increments neither. ([source](../../layer_2_price_reactions.md)).
- [ ] **L2-AC02** — Index assignment survives late-session termination and idempotent replay; related-zone counts do not silently merge. ([source](../../layer_2_price_reactions.md)).
- [ ] **L2-AC03** — Upper/lower boundary equality is handled symmetrically; a whole-zone gap is not a traded touch. ([source](../../layer_2_price_reactions.md)).
- [ ] **L2-AC08** — A resolution candle does not count toward re-arming; side changes cannot complete a mixed-side streak. ([source](../../layer_2_price_reactions.md)).
- [ ] **L2-AC09** — Stalls resolve on the 11th/6th consecutive inside close, not on the 10th/5th. ([source](../../layer_2_price_reactions.md)).
- [ ] **L2-AC17** — The 1m and 5m paths remain independent; shared-boundary observations do not generate duplicate ownership events. ([source](../../layer_2_price_reactions.md)).

Shared cases require evidence from all listed owners in the traceability checklist. Do not close a shared acceptance case based only on this task's portion.

## Deliverables

Episode state machine, exact-scope counters and interaction ownership tests.

Implement only within the new `spx-gex-agent` application unless a separate instruction authorizes otherwise. Record actual implementation/test paths when work begins; this plan does not prescribe unreviewed module names.

## Scope limits

No absorption inference, lineage counter merging or provisional intrabar path.

## Evidence and handoff

| Evidence | Record |
|---|---|
| Implementation / deliverable paths | Not started |
| Test or verification commands | Not run |
| Results and coverage, where applicable | Not run |
| Fixture / session / provider / configuration versions | Not recorded |
| Acceptance-case evidence | Not recorded |
| Review and unresolved findings | Not reviewed |
| Completion date / commit, if applicable | Not completed |

## Blockers and decisions

Not assessed yet. No prerequisite is presumed verified merely because the specification is frozen. Record the specific missing input, owner and affected checks here if a blocker is found.

