# L2-01 — Causal ATR warm-up and frozen buffers

**Status:** NOT_STARTED  
**Group:** layer_2  
**Task type:** Implementation  
**Owner:** Unassigned  
**Created:** 2026-10-29  
**Specification baseline:** Frozen v1.0

[Master checklist](../../TASKS.md) · [Requirements traceability](../TRACEABILITY.md)

## Objective

Produce repeatable frozen ATR evidence for episode and watch thresholds.

## Completion checklist

- [ ] Implementation or deliverable completed
- [ ] Tests / verification checks passed
- [ ] Acceptance criteria verified
- [ ] Reviewed and evidence recorded
- [ ] Complete — master index updated

Every box starts unchecked. Creating this plan is not implementation or testing. Update this task and its master-index entry together; do not mark Complete until required checks and acceptance evidence are recorded. A verification task completes its stated evidence deliverable rather than claiming application code was implemented.

## Authoritative requirements

- [Layer 2 specification](../../layer_2_price_reactions.md) — sections 3, 4.

The linked frozen specifications take precedence over this task summary. Do not silently introduce new strategy rules. Record any material unresolved input or implementation decision below before dependent work proceeds.

## Dependencies

- [ ] [F03 — Source adapters, calendar and completed candles](../foundation/F03-source-adapters-and-candle-normalization.md) completed, or an explicitly documented interface prerequisite is available.

Dependencies describe prerequisites, not permission to change another task's scope. Tests may use agreed fixtures/interfaces before full integration; acceptance still requires the actual dependencies and relevant integration checks.

## Implementation / deliverable checklist

- [ ] Implement true range on completed 15m RTH bars with the documented prior-session official close.
- [ ] Use the latest 130 scheduled complete bars, seed from the first 14 true ranges and apply Wilder recurrence.
- [ ] Select only ATR available by the trigger candle's opening; freeze its window, reference and D/E buffers.
- [ ] Fail explicitly on insufficient/missing inputs; account for holidays and early closes without filling missing bars.

## Required tests and verification

- [ ] Compare seed/recurrence with a hand-calculated fixture including an overnight gap.
- [ ] Test 129 versus 130 bars, missing prior close, missing expected interval and non-finite inputs.
- [ ] Verify D=max(3,0.35*ATR), E=max(2,0.20*ATR), unchanged inherited watch thresholds and causal reference timing.

Run focused tests for this task and relevant integration/regression checks. Record actual commands, versions and results; an unexecuted test plan is not a passing result. Follow applicable repository testing and change-process instructions during implementation.

## Assigned frozen acceptance cases

- [ ] **L2-AC07** — ATR uses the recorded 130-bar seed/window and prior close; missing inputs do not activate point-floor fallback. ([source](../../layer_2_price_reactions.md)).
- [ ] **L2-AC16** — Current health uses a current snapshot, while geometry/ATR remain frozen; unavailable historical inputs cannot be borrowed from the future. ([source](../../layer_2_price_reactions.md)).

Shared cases require evidence from all listed owners in the traceability checklist. Do not close a shared acceptance case based only on this task's portion.

## Deliverables

ATR value object/calculator, warm-up provenance and threshold tests.

Implement only within the new `spx-gex-agent` application unless a separate instruction authorizes otherwise. Record actual implementation/test paths when work begins; this plan does not prescribe unreviewed module names.

## Scope limits

No fallback to point floors when ATR is unavailable or recalculation of active thresholds.

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

