# P07 — Durable outbox and display deduplication

**Status:** NOT_STARTED  
**Group:** storage  
**Task type:** Implementation  
**Owner:** Unassigned  
**Created:** 2026-10-29  
**Specification baseline:** Frozen v1.0

[Master checklist](../../TASKS.md) · [Requirements traceability](../TRACEABILITY.md)

## Objective

Publish committed candidate events without creating duplicate logical candidates.

## Completion checklist

- [ ] Implementation or deliverable completed
- [ ] Tests / verification checks passed
- [ ] Acceptance criteria verified
- [ ] Reviewed and evidence recorded
- [ ] Complete — master index updated

Every box starts unchecked. Creating this plan is not implementation or testing. Update this task and its master-index entry together; do not mark Complete until required checks and acceptance evidence are recorded. A verification task completes its stated evidence deliverable rather than claiming application code was implemented.

## Authoritative requirements

- [Persistence specification](../../persistent_state_and_storage.md) — sections 8, 10, 11, 12.

The linked frozen specifications take precedence over this task summary. Do not silently introduce new strategy rules. Record any material unresolved input or implementation decision below before dependent work proceeds.

## Dependencies

- [ ] [P03 — Single-writer ownership and read-only access](../storage/P03-single-writer-and-readonly-viewers.md) completed, or an explicitly documented interface prerequisite is available.
- [ ] [P05 — Atomic transitions, projections and checkpoints](../storage/P05-atomic-transitions-and-checkpoints.md) completed, or an explicitly documented interface prerequisite is available.

Dependencies describe prerequisites, not permission to change another task's scope. Tests may use agreed fixtures/interfaces before full integration; acceptance still requires the actual dependencies and relevant integration checks.

## Implementation / deliverable checklist

- [ ] Drain durable delivery intents by stable candidate/event ID and persist attempt/ack evidence.
- [ ] Separate delivery purpose and live/recovered/historical presentation; do not relabel old events as fresh after restart.
- [ ] Document retryable delivery and consumer deduplication rather than claiming exactly-once console output.
- [ ] Provide a local display consumer interface with no external messaging or broker execution.

## Required tests and verification

- [ ] Fail after commit but before delivery and verify the original intent is recoverable.
- [ ] Fail after display but before acknowledgment and verify any redelivery carries the same identity.
- [ ] Verify replay streams cannot create live delivery intents and viewer refresh cannot insert candidates.

Run focused tests for this task and relevant integration/regression checks. Record actual commands, versions and results; an unexecuted test plan is not a passing result. Follow applicable repository testing and change-process instructions during implementation.

## Assigned frozen acceptance cases

- [ ] **P-AC05** — A crash after display but before acknowledgment may redeliver the same stable event ID, never create a new candidate or fabricate exactly-once delivery. ([source](../../persistent_state_and_storage.md)).

Shared cases require evidence from all listed owners in the traceability checklist. Do not close a shared acceptance case based only on this task's portion.

## Deliverables

Local delivery/outbox handling and deduplication tests.

Implement only within the new `spx-gex-agent` application unless a separate instruction authorizes otherwise. Record actual implementation/test paths when work begins; this plan does not prescribe unreviewed module names.

## Scope limits

No Slack/email notifications, trade orders or fill inference.

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

