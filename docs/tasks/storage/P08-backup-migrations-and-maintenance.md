# P08 — Backup, migration and restore operations

**Status:** NOT_STARTED  
**Group:** storage  
**Task type:** Implementation  
**Owner:** Unassigned  
**Created:** 2026-10-29  
**Specification baseline:** Frozen v1.0

[Master checklist](../../TASKS.md) · [Requirements traceability](../TRACEABILITY.md)

## Objective

Make schema changes and backups recoverable without dropping causal evidence.

## Completion checklist

- [ ] Implementation or deliverable completed
- [ ] Tests / verification checks passed
- [ ] Acceptance criteria verified
- [ ] Reviewed and evidence recorded
- [ ] Complete — master index updated

Every box starts unchecked. Creating this plan is not implementation or testing. Update this task and its master-index entry together; do not mark Complete until required checks and acceptance evidence are recorded. A verification task completes its stated evidence deliverable rather than claiming application code was implemented.

## Authoritative requirements

- [Persistence specification](../../persistent_state_and_storage.md) — sections 2, 5, 13, 14.

The linked frozen specifications take precedence over this task summary. Do not silently introduce new strategy rules. Record any material unresolved input or implementation decision below before dependent work proceeds.

## Dependencies

- [ ] [P03 — Single-writer ownership and read-only access](../storage/P03-single-writer-and-readonly-viewers.md) completed, or an explicitly documented interface prerequisite is available.
- [ ] [P04 — Zone, state, candidate and evaluation schema](../storage/P04-domain-and-evaluation-schema.md) completed, or an explicitly documented interface prerequisite is available.

Dependencies describe prerequisites, not permission to change another task's scope. Tests may use agreed fixtures/interfaces before full integration; acceptance still requires the actual dependencies and relevant integration checks.

## Implementation / deliverable checklist

- [ ] Implement exclusive-owner migration checks and pre-migration backup with immutable version/checksum history.
- [ ] Use supported SQLite consistent backup procedures and define a restore verification workflow.
- [ ] Add integrity/foreign-key checks, index verification and compatibility errors; preserve journal/evidence dependencies.
- [ ] Document WAL/reader maintenance behavior and avoid implicit retention or destructive cleanup.

## Required tests and verification

- [ ] Back up an active fixture store and restore it into an isolated path.
- [ ] Verify restored foreign keys, source references, journal and projection inputs; reject incompatible migration versions.
- [ ] Inject migration failure and verify a recoverable database/backup remains.

Run focused tests for this task and relevant integration/regression checks. Record actual commands, versions and results; an unexecuted test plan is not a passing result. Follow applicable repository testing and change-process instructions during implementation.

## Assigned frozen acceptance cases

- [ ] **P-AC12** — A restore can rebuild projections, follow foreign-key links and recompute stored decision evidence without reading future outcomes. ([source](../../persistent_state_and_storage.md)).

Shared cases require evidence from all listed owners in the traceability checklist. Do not close a shared acceptance case based only on this task's portion.

## Deliverables

Maintenance commands/workflow, backup/restore documentation and restore tests.

Implement only within the new `spx-gex-agent` application unless a separate instruction authorizes otherwise. Record actual implementation/test paths when work begins; this plan does not prescribe unreviewed module names.

## Scope limits

No automatic historical-data deletion or live database-file copying that ignores WAL.

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

