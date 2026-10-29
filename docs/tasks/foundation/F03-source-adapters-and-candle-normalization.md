# F03 — Source adapters, calendar and completed candles

**Status:** NOT_STARTED  
**Group:** foundation  
**Task type:** Implementation  
**Owner:** Unassigned  
**Created:** 2026-10-29  
**Specification baseline:** Frozen v1.0

[Master checklist](../../TASKS.md) · [Requirements traceability](../TRACEABILITY.md)

## Objective

Publish normalized causal source observations and complete session-aligned candles.

## Completion checklist

- [ ] Implementation or deliverable completed
- [ ] Tests / verification checks passed
- [ ] Acceptance criteria verified
- [ ] Reviewed and evidence recorded
- [ ] Complete — master index updated

Every box starts unchecked. Creating this plan is not implementation or testing. Update this task and its master-index entry together; do not mark Complete until required checks and acceptance evidence are recorded. A verification task completes its stated evidence deliverable rather than claiming application code was implemented.

## Authoritative requirements

- [Layer 1 specification](../../layer_1_zone_discovery.md) — sections 6, 7.
- [Layer 2 specification](../../layer_2_price_reactions.md) — sections 2, 4.
- [Layer 3 specification](../../layer_3_environmental_context.md) — sections 6, 7, 9, 10, 11.
- [Persistence specification](../../persistent_state_and_storage.md) — sections 4, 5.

The linked frozen specifications take precedence over this task summary. Do not silently introduce new strategy rules. Record any material unresolved input or implementation decision below before dependent work proceeds.

## Dependencies

- [ ] [F01 — Project contracts, configuration and test harness](../foundation/F01-project-contracts-and-test-harness.md) completed, or an explicitly documented interface prerequisite is available.
- [ ] [F02 — Provider capabilities and reusable-code audit](../foundation/F02-provider-and-reuse-verification.md) completed, or an explicitly documented interface prerequisite is available.
- [ ] [P02 — Journal ingestion, publication and causal reads](../storage/P02-journal-ingestion-and-asof-reads.md) completed, or an explicitly documented interface prerequisite is available.

Dependencies describe prerequisites, not permission to change another task's scope. Tests may use agreed fixtures/interfaces before full integration; acceptance still requires the actual dependencies and relevant integration checks.

## Implementation / deliverable checklist

- [ ] Implement verified read-only provider adapters for source bars/spot and publication metadata; use storage ingestion rather than direct strategy SQL.
- [ ] Normalize source/availability times and revisions; build completed 1m, 5m and 15m RTH inputs anchored at 09:30.
- [ ] Require constituent completeness and preserve initial partial minutes, gaps, malformed/placeholder inputs and exact source references.
- [ ] Implement session-calendar behavior for holidays, early closes, DST and official prior-close provenance; make historical backfill visibly distinct from live-known evidence.

## Required tests and verification

- [ ] Check exact boundary aggregation, missing constituent minutes, malformed OHLC, duplicate revisions and partial startup candles.
- [ ] Exercise normal/early-close sessions, holidays, DST and prior-session reference selection.
- [ ] Verify late corrections do not replace previously available bars and publication respects event cursors.

Run focused tests for this task and relevant integration/regression checks. Record actual commands, versions and results; an unexecuted test plan is not a passing result. Follow applicable repository testing and change-process instructions during implementation.

## Assigned frozen acceptance cases

No individually numbered source acceptance case is assigned exclusively to this task. Its section requirements and required checks above are mandatory; cross-layer verification is tracked in V03.

Shared cases require evidence from all listed owners in the traceability checklist. Do not close a shared acceptance case based only on this task's portion.

## Deliverables

Normalized source adapters, resampling/calendar utilities, source fixtures and input-publication tests.

Implement only within the new `spx-gex-agent` application unless a separate instruction authorizes otherwise. Record actual implementation/test paths when work begins; this plan does not prescribe unreviewed module names.

## Scope limits

No synthesized bars, silent forward-fills, candidate generation or retrospective live availability.

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

