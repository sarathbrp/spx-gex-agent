# I02 — Read-only monitoring and operator status

**Status:** NOT_STARTED  
**Group:** integration  
**Task type:** Implementation  
**Owner:** Unassigned  
**Created:** 2026-10-29  
**Specification baseline:** Frozen v1.0

[Master checklist](../../TASKS.md) · [Requirements traceability](../TRACEABILITY.md)

## Objective

Make persisted observations and lifecycle state visible without starting another producer.

## Completion checklist

- [ ] Implementation or deliverable completed
- [ ] Tests / verification checks passed
- [ ] Acceptance criteria verified
- [ ] Reviewed and evidence recorded
- [ ] Complete — master index updated

Every box starts unchecked. Creating this plan is not implementation or testing. Update this task and its master-index entry together; do not mark Complete until required checks and acceptance evidence are recorded. A verification task completes its stated evidence deliverable rather than claiming application code was implemented.

## Authoritative requirements

- [Layer 1 specification](../../layer_1_zone_discovery.md) — sections 9, 11.
- [Layer 3 specification](../../layer_3_environmental_context.md) — sections 12.
- [Layer 4 specification](../../layer_4_multitimeframe_alignment.md) — sections 7, 8, 9.
- [Persistence specification](../../persistent_state_and_storage.md) — sections 2, 3, 11, 13.

The linked frozen specifications take precedence over this task summary. Do not silently introduce new strategy rules. Record any material unresolved input or implementation decision below before dependent work proceeds.

## Dependencies

- [ ] [I01 — Live engine orchestration and session scheduling](../integration/I01-live-engine-orchestration.md) completed, or an explicitly documented interface prerequisite is available.

Dependencies describe prerequisites, not permission to change another task's scope. Tests may use agreed fixtures/interfaces before full integration; acceptance still requires the actual dependencies and relevant integration checks.

## Implementation / deliverable checklist

- [ ] Expose read-only queries/minimal viewer for session phase, source health, zones, history completeness, episodes/watches and candidates.
- [ ] Show original environment/MTF evidence separately from later audit and outcomes; label provisional telemetry clearly if exposed.
- [ ] Render stable event identities and live/recovered/historical distinctions without deducing fills or profitability.
- [ ] Document viewer/engine launch behavior and keep any rich TUI layout or framework choice as separate UI design.

## Required tests and verification

- [ ] Open multiple viewers and verify no additional polling, state transitions or candidate inserts.
- [ ] Verify repeated refresh deduplication, missing-versus-zero display and separation of original versus later context.
- [ ] Check viewer write rejection and bounded read transactions while the engine commits.

Run focused tests for this task and relevant integration/regression checks. Record actual commands, versions and results; an unexecuted test plan is not a passing result. Follow applicable repository testing and change-process instructions during implementation.

## Assigned frozen acceptance cases

No individually numbered source acceptance case is assigned exclusively to this task. Its section requirements and required checks above are mandatory; cross-layer verification is tracked in V03.

Shared cases require evidence from all listed owners in the traceability checklist. Do not close a shared acceptance case based only on this task's portion.

## Deliverables

Read-only query/view interface and operator usage instructions.

Implement only within the new `spx-gex-agent` application unless a separate instruction authorizes otherwise. Record actual implementation/test paths when work begins; this plan does not prescribe unreviewed module names.

## Scope limits

No unsolicited UI redesign, mandatory framework, trading controls or execution buttons.

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

