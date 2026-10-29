# F02 — Provider capabilities and reusable-code audit

**Status:** NOT_STARTED  
**Group:** foundation  
**Task type:** Verification / evidence  
**Owner:** Unassigned  
**Created:** 2026-10-29  
**Specification baseline:** Frozen v1.0

[Master checklist](../../TASKS.md) · [Requirements traceability](../TRACEABILITY.md)

## Objective

Resolve source and valuation implementation prerequisites using evidence before dependent adapters are accepted.

## Completion checklist

- [ ] Implementation or deliverable completed
- [ ] Tests / verification checks passed
- [ ] Acceptance criteria verified
- [ ] Reviewed and evidence recorded
- [ ] Complete — master index updated

Every box starts unchecked. Creating this plan is not implementation or testing. Update this task and its master-index entry together; do not mark Complete until required checks and acceptance evidence are recorded. A verification task completes its stated evidence deliverable rather than claiming application code was implemented.

## Authoritative requirements

- [Layer 1 specification](../../layer_1_zone_discovery.md) — sections 2, 3.
- [Layer 2 specification](../../layer_2_price_reactions.md) — sections 4.
- [Layer 3 specification](../../layer_3_environmental_context.md) — sections 3, 4, 6, 9.
- [Persistence specification](../../persistent_state_and_storage.md) — sections 5.

The linked frozen specifications take precedence over this task summary. Do not silently introduce new strategy rules. Record any material unresolved input or implementation decision below before dependent work proceeds.

## Dependencies

No task dependencies.

Dependencies describe prerequisites, not permission to change another task's scope. Tests may use agreed fixtures/interfaces before full integration; acceptance still requires the actual dependencies and relevant integration checks.

## Implementation / deliverable checklist

- [ ] Inspect existing adapters read-only and document reusable normalization, history and valuation code; follow the existing project's CodeGraph instructions if examining its indexed code.
- [ ] Verify accessible SPX/SPXW quotes, independent prior-session OI, exact 0DTE expiry identities, official prior close, SPX/VIX/VIX1D bars and SPY volume conventions.
- [ ] Document request parameters, timestamps/availability limitations, entitlement gaps, revisions, raw fixture provenance and credentials-safe configuration.
- [ ] Specify the option valuation/IV/gamma policy and input conventions required by the frozen contracts; surface material unresolved choices for agreement rather than inventing them.

## Required tests and verification

- [ ] Record read-only provider probes or primary-document evidence for each required input, with sanitized samples.
- [ ] Verify OI universe completeness independently of the returned quote payload and distinguish unavailable APIs from zero data.
- [ ] Check units, timestamp zones and volume aggregation conventions against reproducible sample calculations.

Run focused tests for this task and relevant integration/regression checks. Record actual commands, versions and results; an unexecuted test plan is not a passing result. Follow applicable repository testing and change-process instructions during implementation.

## Assigned frozen acceptance cases

No individually numbered source acceptance case is assigned exclusively to this task. Its section requirements and required checks above are mandatory; cross-layer verification is tracked in V03.

Shared cases require evidence from all listed owners in the traceability checklist. Do not close a shared acceptance case based only on this task's portion.

## Deliverables

Provider/reuse assessment, verified input-contract matrix, sanitized fixtures and explicit unresolved prerequisites.

Implement only within the new `spx-gex-agent` application unless a separate instruction authorizes otherwise. Record actual implementation/test paths when work begins; this plan does not prescribe unreviewed module names.

## Scope limits

No purchases, broker access, changes to spx_orb, or claim of historical availability unsupported by evidence.

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

