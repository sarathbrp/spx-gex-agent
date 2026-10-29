# L1-02 — Deterministic clusters, dominance and composition

**Status:** NOT_STARTED  
**Group:** layer_1  
**Task type:** Implementation  
**Owner:** Unassigned  
**Created:** 2026-10-29  
**Specification baseline:** Frozen v1.0

[Master checklist](../../TASKS.md) · [Requirements traceability](../TRACEABILITY.md)

## Objective

Turn eligible local sensitivities into repeatable spatial zones and research tiers.

## Completion checklist

- [ ] Implementation or deliverable completed
- [ ] Tests / verification checks passed
- [ ] Acceptance criteria verified
- [ ] Reviewed and evidence recorded
- [ ] Complete — master index updated

Every box starts unchecked. Creating this plan is not implementation or testing. Update this task and its master-index entry together; do not mark Complete until required checks and acceptance evidence are recorded. A verification task completes its stated evidence deliverable rather than claiming application code was implemented.

## Authoritative requirements

- [Layer 1 specification](../../layer_1_zone_discovery.md) — sections 4, 5, 10.

The linked frozen specifications take precedence over this task summary. Do not silently introduce new strategy rules. Record any material unresolved input or implementation decision below before dependent work proceeds.

## Dependencies

- [ ] [L1-01 — OI universe, contract valuation and local coverage](../layer_1/L1-01-oi-universe-and-gex-validation.md) completed, or an explicitly documented interface prerequisite is available.

Dependencies describe prerequisites, not permission to change another task's scope. Tests may use agreed fixtures/interfaces before full integration; acceptance still requires the actual dependencies and relevant integration checks.

## Implementation / deliverable checklist

- [ ] Implement descending-gross seeds with spot-distance and lower-strike tie-breaks and exact 5-point neighbor eligibility.
- [ ] Absorb at most one unassigned neighbor at >=50% of seed gross; prohibit bridging missing strikes or three-member clusters.
- [ ] Compute +/-2.5 padded bounds, half-open membership, gross/net/bias and inclusive composition/tier endpoints.
- [ ] Retain every cluster, including sub-threshold observations; compute dominance against all valid corridor gross.

## Required tests and verification

- [ ] Cover plateaus, competing neighbors, identical ties and shuffled input ordering.
- [ ] Verify a 4B seed absorbs a 2B neighbor, two members make a 10-point zone and each strike is assigned once.
- [ ] Check +/-0.35 composition, 10%/20% tier boundaries, zero gross and exact shared-boundary membership.

Run focused tests for this task and relevant integration/regression checks. Record actual commands, versions and results; an unexecuted test plan is not a passing result. Follow applicable repository testing and change-process instructions during implementation.

## Assigned frozen acceptance cases

- [ ] **L1-AC02** — Equal-gross seeds and competing neighbors yield repeatable, non-overlapping strike assignments. ([source](../../layer_1_zone_discovery.md)).
- [ ] **L1-AC03** — A 4B seed absorbs a 2B neighbor at the inclusive 50% boundary; no third strike is absorbed. ([source](../../layer_1_zone_discovery.md)).
- [ ] **L1-AC12** — Sub-threshold clusters are retained; independent 1m/5m evaluation cannot access future outcomes. ([source](../../layer_1_zone_discovery.md)).

Shared cases require evidence from all listed owners in the traceability checklist. Do not close a shared acceptance case based only on this task's portion.

## Deliverables

Pure clustering/ranking implementation and deterministic boundary-case fixtures.

Implement only within the new `spx-gex-agent` application unless a separate instruction authorizes otherwise. Record actual implementation/test paths when work begins; this plan does not prescribe unreviewed module names.

## Scope limits

No support/resistance role assignment, fatigue score, first-touch probability or entry action.

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

