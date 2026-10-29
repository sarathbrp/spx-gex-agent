# L1-03 — Immutable zone versions and causal lineage

**Status:** NOT_STARTED  
**Group:** layer_1  
**Task type:** Implementation  
**Owner:** Unassigned  
**Created:** 2026-10-29  
**Specification baseline:** Frozen v1.0

[Master checklist](../../TASKS.md) · [Requirements traceability](../TRACEABILITY.md)

## Objective

Preserve zone geometry and history across refreshes, seed changes, merges and splits.

## Completion checklist

- [ ] Implementation or deliverable completed
- [ ] Tests / verification checks passed
- [ ] Acceptance criteria verified
- [ ] Reviewed and evidence recorded
- [ ] Complete — master index updated

Every box starts unchecked. Creating this plan is not implementation or testing. Update this task and its master-index entry together; do not mark Complete until required checks and acceptance evidence are recorded. A verification task completes its stated evidence deliverable rather than claiming application code was implemented.

## Authoritative requirements

- [Layer 1 specification](../../layer_1_zone_discovery.md) — sections 8, 9, 11.
- [Persistence specification](../../persistent_state_and_storage.md) — sections 6, 7.

The linked frozen specifications take precedence over this task summary. Do not silently introduce new strategy rules. Record any material unresolved input or implementation decision below before dependent work proceeds.

## Dependencies

- [ ] [L1-02 — Deterministic clusters, dominance and composition](../layer_1/L1-02-clustering-dominance-and-composition.md) completed, or an explicitly documented interface prerequisite is available.
- [ ] [P05 — Atomic transitions, projections and checkpoints](../storage/P05-atomic-transitions-and-checkpoints.md) completed, or an explicitly documented interface prerequisite is available.

Dependencies describe prerequisites, not permission to change another task's scope. Tests may use agreed fixtures/interfaces before full integration; acceptance still requires the actual dependencies and relevant integration checks.

## Implementation / deliverable checklist

- [ ] Persist exact geometry, snapshot-specific zone versions and daily lineage with original availability.
- [ ] Link shared-strike predecessors/successors without summing or max-merging test counts.
- [ ] Expose frozen-version references and exact-zone versus related-history views for Layer 2.
- [ ] Keep original events immutable through cluster changes and retain sub-threshold versions for research.

## Required tests and verification

- [ ] Exercise seed flips, same-geometry refresh, merge, split and no-shared-strike cases.
- [ ] Verify a future lineage edge cannot alter an earlier as-of history query.
- [ ] Check replay/repeated snapshots preserve original identities and do not clone active scopes.

Run focused tests for this task and relevant integration/regression checks. Record actual commands, versions and results; an unexecuted test plan is not a passing result. Follow applicable repository testing and change-process instructions during implementation.

## Assigned frozen acceptance cases

- [ ] **L1-AC09** — Refreshes, seed changes, merges and splits preserve original events and frozen setup boundaries. ([source](../../layer_1_zone_discovery.md)).

Shared cases require evidence from all listed owners in the traceability checklist. Do not close a shared acceptance case based only on this task's portion.

## Deliverables

Zone/lineage persistence and causal history queries.

Implement only within the new `spx-gex-agent` application unless a separate instruction authorizes otherwise. Record actual implementation/test paths when work begins; this plan does not prescribe unreviewed module names.

## Scope limits

No rewriting cluster rejections into singleton rejections or silently inherited counters.

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

