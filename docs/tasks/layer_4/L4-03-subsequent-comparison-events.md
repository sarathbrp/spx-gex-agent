# L4-03 — Subsequent MTF events and follow-up deadlines

**Status:** NOT_STARTED  
**Group:** layer_4  
**Task type:** Implementation  
**Owner:** Unassigned  
**Created:** 2026-10-29  
**Specification baseline:** Frozen v1.0

[Master checklist](../../TASKS.md) · [Requirements traceability](../TRACEABILITY.md)

## Objective

Audit later comparisons from all original states without rewriting prior decisions.

## Completion checklist

- [ ] Implementation or deliverable completed
- [ ] Tests / verification checks passed
- [ ] Acceptance criteria verified
- [ ] Reviewed and evidence recorded
- [ ] Complete — master index updated

Every box starts unchecked. Creating this plan is not implementation or testing. Update this task and its master-index entry together; do not mark Complete until required checks and acceptance evidence are recorded. A verification task completes its stated evidence deliverable rather than claiming application code was implemented.

## Authoritative requirements

- [Layer 4 specification](../../layer_4_multitimeframe_alignment.md) — sections 9, 11, 12.

The linked frozen specifications take precedence over this task summary. Do not silently introduce new strategy rules. Record any material unresolved input or implementation decision below before dependent work proceeds.

## Dependencies

- [ ] [L4-02 — Immutable decision-time MTF records](../layer_4/L4-02-decision-time-mtf-records.md) completed, or an explicitly documented interface prerequisite is available.
- [ ] [P05 — Atomic transitions, projections and checkpoints](../storage/P05-atomic-transitions-and-checkpoints.md) completed, or an explicitly documented interface prerequisite is available.

Dependencies describe prerequisites, not permission to change another task's scope. Tests may use agreed fixtures/interfaces before full integration; acceptance still requires the actual dependencies and relevant integration checks.

## Implementation / deliverable checklist

- [ ] Track each candidate for 300 seconds after decision, bounded by session end, using the original deadline and spatial/configuration rules.
- [ ] Record subsequent agreement/conflict/no-match, health and terminal events from all five states, not just pending.
- [ ] Separate genuinely later confirmation from shared-close/earlier confirmation delivered later and beyond-window late audit.
- [ ] Deduplicate primary/counterpart/config/event identities while keeping complete event histories and reciprocal-view counting explicit.

## Required tests and verification

- [ ] Exercise pending->agreement, agreement->conflict and unavailable->later-evidence without changing original rows.
- [ ] Test the exact deadline, expiry, no-match then another match, same-close delivery and session termination.
- [ ] Restart during follow-up and verify no deadline extension, duplicate event or fresh candidate re-emission.

Run focused tests for this task and relevant integration/regression checks. Record actual commands, versions and results; an unexecuted test plan is not a passing result. Follow applicable repository testing and change-process instructions during implementation.

## Assigned frozen acceptance cases

- [ ] **L4-AC11** — Later events follow candidates from every original state, with the original rows immutable. ([source](../../layer_4_multitimeframe_alignment.md)).
- [ ] **L4-AC12** — Subsequent same-close delivery, genuinely later confirmation, beyond-window arrivals and no-match completion remain distinguishable. ([source](../../layer_4_multitimeframe_alignment.md)).

Shared cases require evidence from all listed owners in the traceability checklist. Do not close a shared acceptance case based only on this task's portion.

## Deliverables

MTF follow-up consumer, durable deadline/audit handling and lifecycle tests.

Implement only within the new `spx-gex-agent` application unless a separate instruction authorizes otherwise. Record actual implementation/test paths when work begins; this plan does not prescribe unreviewed module names.

## Scope limits

No delayed fill, automatic candidate cancellation or selective first-favorable-event reporting.

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

