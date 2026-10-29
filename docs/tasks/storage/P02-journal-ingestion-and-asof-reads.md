# P02 — Journal ingestion, publication and causal reads

**Status:** NOT_STARTED  
**Group:** storage  
**Task type:** Implementation  
**Owner:** Unassigned  
**Created:** 2026-10-29  
**Specification baseline:** Frozen v1.0

[Master checklist](../../TASKS.md) · [Requirements traceability](../TRACEABILITY.md)

## Objective

Make complete validated inputs visible once and only to eligible as-of readers.

## Completion checklist

- [ ] Implementation or deliverable completed
- [ ] Tests / verification checks passed
- [ ] Acceptance criteria verified
- [ ] Reviewed and evidence recorded
- [ ] Complete — master index updated

Every box starts unchecked. Creating this plan is not implementation or testing. Update this task and its master-index entry together; do not mark Complete until required checks and acceptance evidence are recorded. A verification task completes its stated evidence deliverable rather than claiming application code was implemented.

## Authoritative requirements

- [Layer 1 specification](../../layer_1_zone_discovery.md) — sections 7, 11.
- [Persistence specification](../../persistent_state_and_storage.md) — sections 3, 4, 5, 10, 11.

The linked frozen specifications take precedence over this task summary. Do not silently introduce new strategy rules. Record any material unresolved input or implementation decision below before dependent work proceeds.

## Dependencies

- [ ] [P01 — SQLite bootstrap, metadata and source schema](../storage/P01-sqlite-bootstrap-and-source-schema.md) completed, or an explicitly documented interface prerequisite is available.

Dependencies describe prerequisites, not permission to change another task's scope. Tests may use agreed fixtures/interfaces before full integration; acceptance still requires the actual dependencies and relevant integration checks.

## Implementation / deliverable checklist

- [ ] Implement record_observation and load_asof through the storage interface with stable idempotency keys and total event ordering.
- [ ] Preserve source, valuation, availability and recorded clocks; prevent staged/partial batches from being usable inputs.
- [ ] Distinguish identical retries from changed payloads and corrections; persist failed refresh health even without a usable snapshot.
- [ ] Reject conflicting evidence under the same identity; never replace immutable rows to hide inconsistency.

## Required tests and verification

- [ ] Retry identical ingestion and verify unchanged logical counts and cursor visibility.
- [ ] Inject failure before publication/commit and verify no partial evidence becomes eligible.
- [ ] Verify later same-timestamp events, newly delivered old bars and corrected revisions cannot leak across a prior cutoff.

Run focused tests for this task and relevant integration/regression checks. Record actual commands, versions and results; an unexecuted test plan is not a passing result. Follow applicable repository testing and change-process instructions during implementation.

## Assigned frozen acceptance cases

- [ ] **P-AC02** — Repeating one captured input or transition creates no duplicate snapshot revision, episode, count, watch, candidate or audit event. ([source](../../persistent_state_and_storage.md)).
- [ ] **P-AC10** — Late corrected bars and snapshots create revisions without changing original input visibility. ([source](../../persistent_state_and_storage.md)).

Shared cases require evidence from all listed owners in the traceability checklist. Do not close a shared acceptance case based only on this task's portion.

## Deliverables

Durable source journal, publication protocol and tested causal read interface.

Implement only within the new `spx-gex-agent` application unless a separate instruction authorizes otherwise. Record actual implementation/test paths when work begins; this plan does not prescribe unreviewed module names.

## Scope limits

No strategy outcomes, uncontrolled SQL access by evaluators or future-outcome reads.

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

