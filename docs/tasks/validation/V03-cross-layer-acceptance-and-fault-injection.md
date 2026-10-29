# V03 — Cross-layer acceptance and failure validation

**Status:** NOT_STARTED  
**Group:** validation  
**Task type:** Verification / evidence  
**Owner:** Unassigned  
**Created:** 2026-10-29  
**Specification baseline:** Frozen v1.0

[Master checklist](../../TASKS.md) · [Requirements traceability](../TRACEABILITY.md)

## Objective

Prove the complete implementation meets every frozen acceptance case under realistic failures.

## Completion checklist

- [ ] Implementation or deliverable completed
- [ ] Tests / verification checks passed
- [ ] Acceptance criteria verified
- [ ] Reviewed and evidence recorded
- [ ] Complete — master index updated

Every box starts unchecked. Creating this plan is not implementation or testing. Update this task and its master-index entry together; do not mark Complete until required checks and acceptance evidence are recorded. A verification task completes its stated evidence deliverable rather than claiming application code was implemented.

## Authoritative requirements

- [Layer 1 specification](../../layer_1_zone_discovery.md) — sections 12.
- [Layer 2 specification](../../layer_2_price_reactions.md) — sections 15.
- [Layer 3 specification](../../layer_3_environmental_context.md) — sections 14.
- [Layer 4 specification](../../layer_4_multitimeframe_alignment.md) — sections 12.
- [Persistence specification](../../persistent_state_and_storage.md) — sections 14.

The linked frozen specifications take precedence over this task summary. Do not silently introduce new strategy rules. Record any material unresolved input or implementation decision below before dependent work proceeds.

## Dependencies

- [ ] [I02 — Read-only monitoring and operator status](../integration/I02-readonly-monitoring-and-operator-status.md) completed, or an explicitly documented interface prerequisite is available.
- [ ] [P08 — Backup, migration and restore operations](../storage/P08-backup-migrations-and-maintenance.md) completed, or an explicitly documented interface prerequisite is available.
- [ ] [V01 — Deterministic recorded-input replay](../validation/V01-deterministic-recorded-input-replay.md) completed, or an explicitly documented interface prerequisite is available.
- [ ] [V02 — Detached outcomes and descriptive reference moves](../validation/V02-detached-outcomes-and-reference-moves.md) completed, or an explicitly documented interface prerequisite is available.

Dependencies describe prerequisites, not permission to change another task's scope. Tests may use agreed fixtures/interfaces before full integration; acceptance still requires the actual dependencies and relevant integration checks.

## Implementation / deliverable checklist

- [ ] Execute all source acceptance cases and the traceability matrix with reproducible fixtures and evidence references.
- [ ] Exercise transaction failures, writer contention, provider omissions, late inputs, restart recovery and backup restoration end-to-end.
- [ ] Compare candidate population with optional Layers 3/4 healthy, unavailable and conflicting.
- [ ] Record defects, rerun affected checks after fixes, and keep any unverified acceptance item open.

## Required tests and verification

- [ ] Run the full automated suite and coverage report; verify the documented project test standard without inventing pass results.
- [ ] Perform process crash/restart and concurrent-viewer tests against an actual SQLite fixture database.
- [ ] Verify journal/foreign-key integrity, causal replay equivalence and isolation from old spx_orb data/code.

Run focused tests for this task and relevant integration/regression checks. Record actual commands, versions and results; an unexecuted test plan is not a passing result. Follow applicable repository testing and change-process instructions during implementation.

## Assigned frozen acceptance cases

No individually numbered source acceptance case is assigned exclusively to this task. Its section requirements and required checks above are mandatory; cross-layer verification is tracked in V03.

Shared cases require evidence from all listed owners in the traceability checklist. Do not close a shared acceptance case based only on this task's portion.

## Deliverables

Acceptance report with test commands, fixture/config versions, results and unresolved failures.

Implement only within the new `spx-gex-agent` application unless a separate instruction authorizes otherwise. Record actual implementation/test paths when work begins; this plan does not prescribe unreviewed module names.

## Scope limits

Test passing does not establish economic profitability or authorize live execution.

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

