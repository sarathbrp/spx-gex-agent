# P04 — Zone, state, candidate and evaluation schema

**Status:** NOT_STARTED  
**Group:** storage  
**Task type:** Implementation  
**Owner:** Unassigned  
**Created:** 2026-10-29  
**Specification baseline:** Frozen v1.0

[Master checklist](../../TASKS.md) · [Requirements traceability](../TRACEABILITY.md)

## Objective

Add relational identities for all four layers without coupling later outcomes to decisions.

## Completion checklist

- [ ] Implementation or deliverable completed
- [ ] Tests / verification checks passed
- [ ] Acceptance criteria verified
- [ ] Reviewed and evidence recorded
- [ ] Complete — master index updated

Every box starts unchecked. Creating this plan is not implementation or testing. Update this task and its master-index entry together; do not mark Complete until required checks and acceptance evidence are recorded. A verification task completes its stated evidence deliverable rather than claiming application code was implemented.

## Authoritative requirements

- [Persistence specification](../../persistent_state_and_storage.md) — sections 6, 7, 8.
- [Layer 1 specification](../../layer_1_zone_discovery.md) — sections 9, 11.
- [Layer 2 specification](../../layer_2_price_reactions.md) — sections 13.
- [Layer 3 specification](../../layer_3_environmental_context.md) — sections 12.
- [Layer 4 specification](../../layer_4_multitimeframe_alignment.md) — sections 8, 9, 12.

The linked frozen specifications take precedence over this task summary. Do not silently introduce new strategy rules. Record any material unresolved input or implementation decision below before dependent work proceeds.

## Dependencies

- [ ] [P01 — SQLite bootstrap, metadata and source schema](../storage/P01-sqlite-bootstrap-and-source-schema.md) completed, or an explicitly documented interface prerequisite is available.

Dependencies describe prerequisites, not permission to change another task's scope. Tests may use agreed fixtures/interfaces before full integration; acceptance still requires the actual dependencies and relevant integration checks.

## Implementation / deliverable checklist

- [ ] Migrate geometry/history/version/lineage, scope/episode/watch, pattern/eligibility/candidate and checkpoint tables.
- [ ] Add immutable environment and MTF records, full match sets, subsequent events, enrichment audit, evaluation results and delivery outbox.
- [ ] Enforce cross-record stream/configuration consistency, one active episode/watch per scope, eligible-decision candidate references and concrete parent references.
- [ ] Keep immutable creation fields separate from mutable projections; use versioned JSON only for verbose evidence, not hidden relational keys.

## Required tests and verification

- [ ] Reject orphan/cross-scope references, duplicate eligible candidates and conflicting active state.
- [ ] Verify multiple predecessor lineages and same-geometry versions retain distinct immutable histories.
- [ ] Check original context cannot be overwritten by subsequent/audit/outcome inserts.

Run focused tests for this task and relevant integration/regression checks. Record actual commands, versions and results; an unexecuted test plan is not a passing result. Follow applicable repository testing and change-process instructions during implementation.

## Assigned frozen acceptance cases

- [ ] **P-AC03** — Two candidates on the same timestamp but different valid scopes remain distinct. ([source](../../persistent_state_and_storage.md)).

Shared cases require evidence from all listed owners in the traceability checklist. Do not close a shared acceptance case based only on this task's portion.

## Deliverables

Domain and detached-evaluation migrations with constraint and serialization tests.

Implement only within the new `spx-gex-agent` application unless a separate instruction authorizes otherwise. Record actual implementation/test paths when work begins; this plan does not prescribe unreviewed module names.

## Scope limits

No SQS, portfolio or fill schema; no unconstrained polymorphic references.

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

