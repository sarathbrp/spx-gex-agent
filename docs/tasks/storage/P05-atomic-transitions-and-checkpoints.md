# P05 — Atomic transitions, projections and checkpoints

**Status:** NOT_STARTED  
**Group:** storage  
**Task type:** Implementation  
**Owner:** Unassigned  
**Created:** 2026-10-29  
**Specification baseline:** Frozen v1.0

[Master checklist](../../TASKS.md) · [Requirements traceability](../TRACEABILITY.md)

## Objective

Commit each semantic state transition and its durable outputs exactly once within the database.

## Completion checklist

- [ ] Implementation or deliverable completed
- [ ] Tests / verification checks passed
- [ ] Acceptance criteria verified
- [ ] Reviewed and evidence recorded
- [ ] Complete — master index updated

Every box starts unchecked. Creating this plan is not implementation or testing. Update this task and its master-index entry together; do not mark Complete until required checks and acceptance evidence are recorded. A verification task completes its stated evidence deliverable rather than claiming application code was implemented.

## Authoritative requirements

- [Layer 1 specification](../../layer_1_zone_discovery.md) — sections 11.
- [Layer 2 specification](../../layer_2_price_reactions.md) — sections 5, 13.
- [Persistence specification](../../persistent_state_and_storage.md) — sections 3, 7, 9, 10, 11.

The linked frozen specifications take precedence over this task summary. Do not silently introduce new strategy rules. Record any material unresolved input or implementation decision below before dependent work proceeds.

## Dependencies

- [ ] [P02 — Journal ingestion, publication and causal reads](../storage/P02-journal-ingestion-and-asof-reads.md) completed, or an explicitly documented interface prerequisite is available.
- [ ] [P04 — Zone, state, candidate and evaluation schema](../storage/P04-domain-and-evaluation-schema.md) completed, or an explicitly documented interface prerequisite is available.

Dependencies describe prerequisites, not permission to change another task's scope. Tests may use agreed fixtures/interfaces before full integration; acceptance still requires the actual dependencies and relevant integration checks.

## Implementation / deliverable checklist

- [ ] Implement commit_transition with expected cursor/state-version checks and short transactions.
- [ ] Atomically append transition evidence, immutable decisions/candidates, projection updates, terminal consumption, checkpoint and delivery intent.
- [ ] Retain frozen zone/ATR fields and original deadlines; record clock/deadline inputs needed for replay.
- [ ] Make retries return identical committed results and flag same-key/different-evidence conflicts.

## Required tests and verification

- [ ] Inject failures at every transaction stage and verify all-or-nothing state, candidate, checkpoint and outbox writes.
- [ ] Retry committed transitions and confirm no extra test index, candidate or terminal event.
- [ ] Reject stale expected cursors and verify optional-context failure cannot roll back a candidate.

Run focused tests for this task and relevant integration/regression checks. Record actual commands, versions and results; an unexecuted test plan is not a passing result. Follow applicable repository testing and change-process instructions during implementation.

## Assigned frozen acceptance cases

- [ ] **P-AC02** — Repeating one captured input or transition creates no duplicate snapshot revision, episode, count, watch, candidate or audit event. ([source](../../persistent_state_and_storage.md)).
- [ ] **P-AC04** — A crash before transition commit leaves no partial candidate, advanced checkpoint or consumed watch. A crash after commit restores all of them together. ([source](../../persistent_state_and_storage.md)).

Shared cases require evidence from all listed owners in the traceability checklist. Do not close a shared acceptance case based only on this task's portion.

## Deliverables

Transactional state-commit interface, projection handling and failure-injection tests.

Implement only within the new `spx-gex-agent` application unless a separate instruction authorizes otherwise. Record actual implementation/test paths when work begins; this plan does not prescribe unreviewed module names.

## Scope limits

No external notification before commit or exactly-once network/display guarantee.

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

