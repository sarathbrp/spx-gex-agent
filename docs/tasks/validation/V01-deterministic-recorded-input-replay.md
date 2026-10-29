# V01 — Deterministic recorded-input replay

**Status:** NOT_STARTED  
**Group:** validation  
**Task type:** Implementation  
**Owner:** Unassigned  
**Created:** 2026-10-29  
**Specification baseline:** Frozen v1.0

[Master checklist](../../TASKS.md) · [Requirements traceability](../TRACEABILITY.md)

## Objective

Verify that recorded causal inputs reproduce semantic results in an isolated replay stream.

## Completion checklist

- [ ] Implementation or deliverable completed
- [ ] Tests / verification checks passed
- [ ] Acceptance criteria verified
- [ ] Reviewed and evidence recorded
- [ ] Complete — master index updated

Every box starts unchecked. Creating this plan is not implementation or testing. Update this task and its master-index entry together; do not mark Complete until required checks and acceptance evidence are recorded. A verification task completes its stated evidence deliverable rather than claiming application code was implemented.

## Authoritative requirements

- [Layer 1 specification](../../layer_1_zone_discovery.md) — sections 7, 11.
- [Layer 2 specification](../../layer_2_price_reactions.md) — sections 2, 12, 13.
- [Layer 3 specification](../../layer_3_environmental_context.md) — sections 2, 13.
- [Layer 4 specification](../../layer_4_multitimeframe_alignment.md) — sections 3, 8, 12.
- [Persistence specification](../../persistent_state_and_storage.md) — sections 4, 12, 13.

The linked frozen specifications take precedence over this task summary. Do not silently introduce new strategy rules. Record any material unresolved input or implementation decision below before dependent work proceeds.

## Dependencies

- [ ] [P06 — Restart recovery and state reconciliation](../storage/P06-restart-recovery-and-reconciliation.md) completed, or an explicitly documented interface prerequisite is available.

Dependencies describe prerequisites, not permission to change another task's scope. Tests may use agreed fixtures/interfaces before full integration; acceptance still requires the actual dependencies and relevant integration checks.

## Implementation / deliverable checklist

- [ ] Replay original source revisions, publication order, decision schedule and clock events through the same layer logic.
- [ ] Separate exact recorded-input replay from retrospective provider-history reconstruction.
- [ ] Write replay outputs in their own namespace/database and prohibit live outbox delivery.
- [ ] Compare semantic events, statuses, scopes and counts while accounting for namespaced replay IDs.

## Required tests and verification

- [ ] Compare uninterrupted and restarted replay results over the same recorded fixture.
- [ ] Exercise same-close ordering, late data, gap repairs, session rollover and frozen deadlines.
- [ ] Run replay twice and verify deterministic semantic equivalence, no live writes and no outcome access.

Run focused tests for this task and relevant integration/regression checks. Record actual commands, versions and results; an unexecuted test plan is not a passing result. Follow applicable repository testing and change-process instructions during implementation.

## Assigned frozen acceptance cases

- [ ] **P-AC08** — Same-close 1m/5m races reproduce original comparison states from event ordering; later input cannot overwrite them. ([source](../../persistent_state_and_storage.md)).
- [ ] **P-AC11** — A replay writes only to its own namespace and cannot generate live delivery intents. ([source](../../persistent_state_and_storage.md)).

Shared cases require evidence from all listed owners in the traceability checklist. Do not close a shared acceptance case based only on this task's portion.

## Deliverables

Replay command/workflow, semantic comparison report and repeatability fixtures.

Implement only within the new `spx-gex-agent` application unless a separate instruction authorizes otherwise. Record actual implementation/test paths when work begins; this plan does not prescribe unreviewed module names.

## Scope limits

No historical-as-live claims where availability was not recorded or retroactive correction of live decisions.

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

