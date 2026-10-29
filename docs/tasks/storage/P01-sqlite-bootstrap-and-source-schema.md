# P01 — SQLite bootstrap, metadata and source schema

**Status:** NOT_STARTED  
**Group:** storage  
**Task type:** Implementation  
**Owner:** Unassigned  
**Created:** 2026-10-29  
**Specification baseline:** Frozen v1.0

[Master checklist](../../TASKS.md) · [Requirements traceability](../TRACEABILITY.md)

## Objective

Establish the versioned SQLite store and relational source evidence model.

## Completion checklist

- [ ] Implementation or deliverable completed
- [ ] Tests / verification checks passed
- [ ] Acceptance criteria verified
- [ ] Reviewed and evidence recorded
- [ ] Complete — master index updated

Every box starts unchecked. Creating this plan is not implementation or testing. Update this task and its master-index entry together; do not mark Complete until required checks and acceptance evidence are recorded. A verification task completes its stated evidence deliverable rather than claiming application code was implemented.

## Authoritative requirements

- [Persistence specification](../../persistent_state_and_storage.md) — sections 2, 4, 5, 13.

The linked frozen specifications take precedence over this task summary. Do not silently introduce new strategy rules. Record any material unresolved input or implementation decision below before dependent work proceeds.

## Dependencies

- [ ] [F01 — Project contracts, configuration and test harness](../foundation/F01-project-contracts-and-test-harness.md) completed, or an explicitly documented interface prerequisite is available.

Dependencies describe prerequisites, not permission to change another task's scope. Tests may use agreed fixtures/interfaces before full integration; acceptance still requires the actual dependencies and relevant integration checks.

## Implementation / deliverable checklist

- [ ] Create initial migrations for schema/config/stream/process metadata, ingest batches, journal, bars, OI baseline/contracts and GEX snapshots/contracts.
- [ ] Configure WAL, FULL synchronization, per-connection foreign keys and bounded busy handling; verify runtime support.
- [ ] Encode canonical keys, source/publication references, independent LOCAL/BROAD health and revision uniqueness.
- [ ] Record migration checksums and compatibility; reserve operational migration ownership and backup coordination for P08.

## Required tests and verification

- [ ] Create an empty database and reopen it with compatible schema/configuration.
- [ ] Verify foreign keys and logical uniqueness reject invalid inserts while preserving genuine revisions and distinct same-time scopes.
- [ ] Verify pragma enforcement, unavailable values and local/broad quality separation.

Run focused tests for this task and relevant integration/regression checks. Record actual commands, versions and results; an unexecuted test plan is not a passing result. Follow applicable repository testing and change-process instructions during implementation.

## Assigned frozen acceptance cases

No individually numbered source acceptance case is assigned exclusively to this task. Its section requirements and required checks above are mandatory; cross-layer verification is tracked in V03.

Shared cases require evidence from all listed owners in the traceability checklist. Do not close a shared acceptance case based only on this task's portion.

## Deliverables

Initial metadata/source migrations and schema validation tests.

Implement only within the new `spx-gex-agent` application unless a separate instruction authorizes otherwise. Record actual implementation/test paths when work begins; this plan does not prescribe unreviewed module names.

## Scope limits

Do not create order/account tables or treat timestamps alone as unique identities.

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

