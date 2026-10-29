# F01 — Project contracts, configuration and test harness

**Status:** NOT_STARTED  
**Group:** foundation  
**Task type:** Implementation  
**Owner:** Unassigned  
**Created:** 2026-10-29  
**Specification baseline:** Frozen v1.0

[Master checklist](../../TASKS.md) · [Requirements traceability](../TRACEABILITY.md)

## Objective

Create the separate application's minimal foundation and typed contracts without altering spx_orb.

## Completion checklist

- [ ] Implementation or deliverable completed
- [ ] Tests / verification checks passed
- [ ] Acceptance criteria verified
- [ ] Reviewed and evidence recorded
- [ ] Complete — master index updated

Every box starts unchecked. Creating this plan is not implementation or testing. Update this task and its master-index entry together; do not mark Complete until required checks and acceptance evidence are recorded. A verification task completes its stated evidence deliverable rather than claiming application code was implemented.

## Authoritative requirements

- [Layer 1 specification](../../layer_1_zone_discovery.md) — sections 1, 2, 6, 7.
- [Layer 2 specification](../../layer_2_price_reactions.md) — sections 2, 3.
- [Persistence specification](../../persistent_state_and_storage.md) — sections 1, 3, 4.

The linked frozen specifications take precedence over this task summary. Do not silently introduce new strategy rules. Record any material unresolved input or implementation decision below before dependent work proceeds.

## Dependencies

No task dependencies.

Dependencies describe prerequisites, not permission to change another task's scope. Tests may use agreed fixtures/interfaces before full integration; acceptance still requires the actual dependencies and relevant integration checks.

## Implementation / deliverable checklist

- [ ] Establish the new app's package/test layout and documented development commands; keep existing spx-agent code and data untouched.
- [ ] Define canonical session/universe/configuration identities, timestamps, event cursors, price encoding and explicit unavailable statuses shared by all layers.
- [ ] Version research defaults and injectable clock/calendar interfaces; establish fixture-driven tests and a coverage command without claiming unapproved numerical coverage targets.
- [ ] Record any remaining language/package/tooling choice before coding; propose the smallest reusable implementation rather than importing the existing ORB strategy wholesale.

## Required tests and verification

- [ ] Validate canonical identities/configuration hashes, invalid parameters and missing-versus-zero representations.
- [ ] Verify timezone-aware times, exact price identities and same-timestamp cursor ordering.
- [ ] Run the new package's smoke test and documented test/coverage commands independently of spx_orb.

Run focused tests for this task and relevant integration/regression checks. Record actual commands, versions and results; an unexecuted test plan is not a passing result. Follow applicable repository testing and change-process instructions during implementation.

## Assigned frozen acceptance cases

No individually numbered source acceptance case is assigned exclusively to this task. Its section requirements and required checks above are mandatory; cross-layer verification is tracked in V03.

Shared cases require evidence from all listed owners in the traceability checklist. Do not close a shared acceptance case based only on this task's portion.

## Deliverables

Package foundation, typed contracts, parameter manifest, fixture helpers and reproducible test commands.

Implement only within the new `spx-gex-agent` application unless a separate instruction authorizes otherwise. Record actual implementation/test paths when work begins; this plan does not prescribe unreviewed module names.

## Scope limits

No strategy implementation, broker integration, score formula or new UI framework.

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

