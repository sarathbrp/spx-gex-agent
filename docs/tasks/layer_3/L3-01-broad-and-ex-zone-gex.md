# L3-01 — Broad GEX and synchronized ex-zone context

**Status:** NOT_STARTED  
**Group:** layer_3  
**Task type:** Implementation  
**Owner:** Unassigned  
**Created:** 2026-10-29  
**Specification baseline:** Frozen v1.0

[Master checklist](../../TASKS.md) · [Requirements traceability](../TRACEABILITY.md)

## Objective

Measure broader exposure without double-counting frozen dollar values or blocking local candidates.

## Completion checklist

- [ ] Implementation or deliverable completed
- [ ] Tests / verification checks passed
- [ ] Acceptance criteria verified
- [ ] Reviewed and evidence recorded
- [ ] Complete — master index updated

Every box starts unchecked. Creating this plan is not implementation or testing. Update this task and its master-index entry together; do not mark Complete until required checks and acceptance evidence are recorded. A verification task completes its stated evidence deliverable rather than claiming application code was implemented.

## Authoritative requirements

- [Layer 3 specification](../../layer_3_environmental_context.md) — sections 3, 4, 5.

The linked frozen specifications take precedence over this task summary. Do not silently introduce new strategy rules. Record any material unresolved input or implementation decision below before dependent work proceeds.

## Dependencies

- [ ] [L1-01 — OI universe, contract valuation and local coverage](../layer_1/L1-01-oi-universe-and-gex-validation.md) completed, or an explicitly documented interface prerequisite is available.
- [ ] [P04 — Zone, state, candidate and evaluation schema](../storage/P04-domain-and-evaluation-schema.md) completed, or an explicitly documented interface prerequisite is available.

Dependencies describe prerequisites, not permission to change another task's scope. Tests may use agreed fixtures/interfaces before full integration; acceptance still requires the actual dependencies and relevant integration checks.

## Implementation / deliverable checklist

- [ ] Build the same-expiry +/-150 snapshot using the independent OI baseline and inclusive 85% broad-quality threshold.
- [ ] Compute signed net/gross/balance, same-snapshot candidate share/bias and ex-zone balance; retain frozen bias separately.
- [ ] Persist subgroup coverage and missing/out-of-corridor member reasons with null undefined comparisons.
- [ ] Preserve independent local 90% health and broad availability, snapshot times and model assumptions.

## Required tests and verification

- [ ] Verify G=100,N=-20,Gc=20,Nc=10 yields -0.20,0.20,+0.50 and -0.375.
- [ ] Test exact85%, zero denominators versus zero net, missing local members and members outside the moving broad corridor.
- [ ] Assert broad failure does not modify otherwise valid local candidate eligibility or synthesize neutral metrics.

Run focused tests for this task and relevant integration/regression checks. Record actual commands, versions and results; an unexecuted test plan is not a passing result. Follow applicable repository testing and change-process instructions during implementation.

## Assigned frozen acceptance cases

- [ ] **L3-AC02** — Broad coverage of exactly 85% passes; below 85% nulls broad environmental metrics without changing local 90% requirements or candidate eligibility. ([source](../../layer_3_environmental_context.md)).
- [ ] **L3-AC04** — Synchronized zone subtraction uses the broad snapshot, not frozen episode-time dollar values. Out-of-corridor members are explicit. ([source](../../layer_3_environmental_context.md)).
- [ ] **L3-AC05** — G=100, N=-20, G_C=20, N_C=10 produces balance -0.20, share 0.20, synchronized bias +0.50 and ex-zone balance -0.375. ([source](../../layer_3_environmental_context.md)).
- [ ] **L3-AC06** — Zero gross denominators yield null ratios; zero net with positive gross yields valid zero. Unavailable inputs never become a congruent label. ([source](../../layer_3_environmental_context.md)).
- [ ] **P-AC07** — A snapshot with missing contracts retains the independent OI denominator and failure evidence; local and broad health stay separate. ([source](../../persistent_state_and_storage.md)).

Shared cases require evidence from all listed owners in the traceability checklist. Do not close a shared acceptance case based only on this task's portion.

## Deliverables

Broad/ex-zone metrics builder and synchronized-comparison fixtures.

Implement only within the new `spx-gex-agent` application unless a separate instruction authorizes otherwise. Record actual implementation/test paths when work begins; this plan does not prescribe unreviewed module names.

## Scope limits

No claim of 95% total exposure capture, broad-data entry veto or arbitrary regime bonus.

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

