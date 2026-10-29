# P03 — Single-writer ownership and read-only access

**Status:** NOT_STARTED  
**Group:** storage  
**Task type:** Implementation  
**Owner:** Unassigned  
**Created:** 2026-10-29  
**Specification baseline:** Frozen v1.0

[Master checklist](../../TASKS.md) · [Requirements traceability](../TRACEABILITY.md)

## Objective

Ensure two monitor processes cannot independently write or evaluate the same store.

## Completion checklist

- [ ] Implementation or deliverable completed
- [ ] Tests / verification checks passed
- [ ] Acceptance criteria verified
- [ ] Reviewed and evidence recorded
- [ ] Complete — master index updated

Every box starts unchecked. Creating this plan is not implementation or testing. Update this task and its master-index entry together; do not mark Complete until required checks and acceptance evidence are recorded. A verification task completes its stated evidence deliverable rather than claiming application code was implemented.

## Authoritative requirements

- [Persistence specification](../../persistent_state_and_storage.md) — sections 2, 11, 12, 13.

The linked frozen specifications take precedence over this task summary. Do not silently introduce new strategy rules. Record any material unresolved input or implementation decision below before dependent work proceeds.

## Dependencies

- [ ] [P01 — SQLite bootstrap, metadata and source schema](../storage/P01-sqlite-bootstrap-and-source-schema.md) completed, or an explicitly documented interface prerequisite is available.

Dependencies describe prerequisites, not permission to change another task's scope. Tests may use agreed fixtures/interfaces before full integration; acceptance still requires the actual dependencies and relevant integration checks.

## Implementation / deliverable checklist

- [ ] Acquire an OS-held lock for canonical database identity before recovery, polling or writes; define supported alias handling.
- [ ] Reject a second engine writer clearly and provide an explicit viewer path without source polling or candidate insertion.
- [ ] Open viewer connections read-only and keep read transactions short; require maintenance to obey writer ownership.
- [ ] Persist process diagnostics separately from stable strategy stream identities; never use heartbeat/PID text as the lock.

## Required tests and verification

- [ ] Use two real processes to verify one writer and read-only concurrent access.
- [ ] Terminate the owner and verify safe lock release/reacquisition without creating a new strategy identity.
- [ ] Exercise supported path aliases and attempted writes through viewer connections.

Run focused tests for this task and relevant integration/regression checks. Record actual commands, versions and results; an unexecuted test plan is not a passing result. Follow applicable repository testing and change-process instructions during implementation.

## Assigned frozen acceptance cases

- [ ] **P-AC01** — Starting two engines against the same store yields one writer; extra viewer sessions produce no additional source polling or candidate rows. ([source](../../persistent_state_and_storage.md)).

Shared cases require evidence from all listed owners in the traceability checklist. Do not close a shared acceptance case based only on this task's portion.

## Deliverables

Writer ownership guard, viewer connection interface and multiprocess contention tests.

Implement only within the new `spx-gex-agent` application unless a separate instruction authorizes otherwise. Record actual implementation/test paths when work begins; this plan does not prescribe unreviewed module names.

## Scope limits

No distributed leases, network-filesystem WAL or automatic competing producer fallback.

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

