# V04 — Live observation validation and operational runbook

**Status:** NOT_STARTED  
**Group:** validation  
**Task type:** Verification / evidence  
**Owner:** Unassigned  
**Created:** 2026-10-29  
**Specification baseline:** Frozen v1.0

[Master checklist](../../TASKS.md) · [Requirements traceability](../TRACEABILITY.md)

## Objective

Validate actual feed-to-persisted-observation behavior and document operation without implying trades.

## Completion checklist

- [ ] Implementation or deliverable completed
- [ ] Tests / verification checks passed
- [ ] Acceptance criteria verified
- [ ] Reviewed and evidence recorded
- [ ] Complete — master index updated

Every box starts unchecked. Creating this plan is not implementation or testing. Update this task and its master-index entry together; do not mark Complete until required checks and acceptance evidence are recorded. A verification task completes its stated evidence deliverable rather than claiming application code was implemented.

## Authoritative requirements

- [Layer 1 specification](../../layer_1_zone_discovery.md) — sections 6, 7, 11.
- [Layer 2 specification](../../layer_2_price_reactions.md) — sections 12, 13.
- [Layer 3 specification](../../layer_3_environmental_context.md) — sections 13, 14.
- [Layer 4 specification](../../layer_4_multitimeframe_alignment.md) — sections 9, 12.
- [Persistence specification](../../persistent_state_and_storage.md) — sections 11, 12, 13, 14.

The linked frozen specifications take precedence over this task summary. Do not silently introduce new strategy rules. Record any material unresolved input or implementation decision below before dependent work proceeds.

## Dependencies

- [ ] [V03 — Cross-layer acceptance and failure validation](../validation/V03-cross-layer-acceptance-and-fault-injection.md) completed, or an explicitly documented interface prerequisite is available.

Dependencies describe prerequisites, not permission to change another task's scope. Tests may use agreed fixtures/interfaces before full integration; acceptance still requires the actual dependencies and relevant integration checks.

## Implementation / deliverable checklist

- [ ] Prepare a runbook for prerequisites, engine/viewer startup, health checks, stop/resume, backup/restore and replay.
- [ ] Capture a real session with actual availability and provenance; verify opening observation, completed candidate evidence and later context when naturally observed.
- [ ] Exercise a controlled restart and extra viewer using the same store, checking stable identities and preserved histories.
- [ ] Record the exact observed setup types, gaps and unobserved scenarios; leave live acceptance pending when market/provider conditions do not permit verification.

## Required tests and verification

- [ ] Trace at least one naturally observed eligible candidate from source inputs through durable Layer 2 evidence and Layer 3/4 status/audit; do not manufacture a signal.
- [ ] Reconcile stored counts/checkpoints before and after restart and verify extra viewer adds no producer or candidate.
- [ ] Attach dated session/config/provider records and runbook results; distinguish fixture coverage from live-observed behavior.

Run focused tests for this task and relevant integration/regression checks. Record actual commands, versions and results; an unexecuted test plan is not a passing result. Follow applicable repository testing and change-process instructions during implementation.

## Assigned frozen acceptance cases

No individually numbered source acceptance case is assigned exclusively to this task. Its section requirements and required checks above are mandatory; cross-layer verification is tracked in V03.

Shared cases require evidence from all listed owners in the traceability checklist. Do not close a shared acceptance case based only on this task's portion.

## Deliverables

Operator runbook and honest live observation report with remaining unobserved cases.

Implement only within the new `spx-gex-agent` application unless a separate instruction authorizes otherwise. Record actual implementation/test paths when work begins; this plan does not prescribe unreviewed module names.

## Scope limits

No broker orders, paper exits, execution-cycle claim or performance-validation claim.

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

