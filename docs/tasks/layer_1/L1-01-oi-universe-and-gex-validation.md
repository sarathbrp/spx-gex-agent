# L1-01 — OI universe, contract valuation and local coverage

**Status:** NOT_STARTED  
**Group:** layer_1  
**Task type:** Implementation  
**Owner:** Unassigned  
**Created:** 2026-10-29  
**Specification baseline:** Frozen v1.0

[Master checklist](../../TASKS.md) · [Requirements traceability](../TRACEABILITY.md)

## Objective

Build a reproducible local GEX snapshot from the independent OI universe and usable contract inputs.

## Completion checklist

- [ ] Implementation or deliverable completed
- [ ] Tests / verification checks passed
- [ ] Acceptance criteria verified
- [ ] Reviewed and evidence recorded
- [ ] Complete — master index updated

Every box starts unchecked. Creating this plan is not implementation or testing. Update this task and its master-index entry together; do not mark Complete until required checks and acceptance evidence are recorded. A verification task completes its stated evidence deliverable rather than claiming application code was implemented.

## Authoritative requirements

- [Layer 1 specification](../../layer_1_zone_discovery.md) — sections 2, 3.
- [Layer 3 specification](../../layer_3_environmental_context.md) — sections 3, 4.
- [Persistence specification](../../persistent_state_and_storage.md) — sections 5.

The linked frozen specifications take precedence over this task summary. Do not silently introduce new strategy rules. Record any material unresolved input or implementation decision below before dependent work proceeds.

## Dependencies

- [ ] [F03 — Source adapters, calendar and completed candles](../foundation/F03-source-adapters-and-candle-normalization.md) completed, or an explicitly documented interface prerequisite is available.

Dependencies describe prerequisites, not permission to change another task's scope. Tests may use agreed fixtures/interfaces before full integration; acceptance still requires the actual dependencies and relevant integration checks.

## Implementation / deliverable checklist

- [ ] Implement verified same-session 0DTE SPXW universe and prior-session OI provenance for the inclusive spot +/-75 corridor.
- [ ] Apply the documented contract valuation policy, per-contract 120-second quote freshness, non-crossed prices and IV bounds [0.01,3.00].
- [ ] Compute signed net and modeled gross sensitivities with explicit missing-side evidence and local OI coverage >=90%.
- [ ] Differentiate incomplete/unavailable baseline, verified zero baseline OI, zero gross exposure and valid diffuse concentration; persist contract exclusion reasons.

## Required tests and verification

- [ ] Verify +3B call/-3B put produces 6B gross and zero net; test zero and missing denominators separately.
- [ ] Check exactly 90% coverage, 120-second freshness and IV endpoints; missing quotes stay in the OI denominator.
- [ ] Verify zero bid never implies zero gamma, omitted sides are not imputed and future quotes are excluded.

Run focused tests for this task and relevant integration/regression checks. Record actual commands, versions and results; an unexecuted test plan is not a passing result. Follow applicable repository testing and change-process instructions during implementation.

## Assigned frozen acceptance cases

- [ ] **L1-AC01** — +3B call and -3B put sensitivity produce 6B gross, zero net and balanced composition. ([source](../../layer_1_zone_discovery.md)).
- [ ] **L1-AC04** — Missing quote contracts remain in the independently established OI denominator; exactly 90% passes coverage. ([source](../../layer_1_zone_discovery.md)).
- [ ] **L1-AC05** — Zero bid is never automatically converted to zero gamma; missing baseline is never zero exposure. ([source](../../layer_1_zone_discovery.md)).
- [ ] **L3-AC03** — Missing independent OI baseline is unavailable, never a zero-exposure observation. Zero bid does not imply zero modeled gamma. ([source](../../layer_3_environmental_context.md)).
- [ ] **P-AC07** — A snapshot with missing contracts retains the independent OI denominator and failure evidence; local and broad health stay separate. ([source](../../persistent_state_and_storage.md)).

Shared cases require evidence from all listed owners in the traceability checklist. Do not close a shared acceptance case based only on this task's portion.

## Deliverables

Validated local snapshot builder with source-backed valuation fixtures and contract-level audit evidence.

Implement only within the new `spx-gex-agent` application unless a separate instruction authorizes otherwise. Record actual implementation/test paths when work begins; this plan does not prescribe unreviewed module names.

## Scope limits

No dealer-inventory certainty, option-selection policy or broad-coverage substitution for local requirements.

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

