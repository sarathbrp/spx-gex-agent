# L1-04 — Snapshot eligibility, phases and zone handoff

**Status:** NOT_STARTED  
**Group:** layer_1  
**Task type:** Implementation  
**Owner:** Unassigned  
**Created:** 2026-10-29  
**Specification baseline:** Frozen v1.0

[Master checklist](../../TASKS.md) · [Requirements traceability](../TRACEABILITY.md)

## Objective

Deliver only causally eligible zone versions while retaining opening-period observations and health failures.

## Completion checklist

- [ ] Implementation or deliverable completed
- [ ] Tests / verification checks passed
- [ ] Acceptance criteria verified
- [ ] Reviewed and evidence recorded
- [ ] Complete — master index updated

Every box starts unchecked. Creating this plan is not implementation or testing. Update this task and its master-index entry together; do not mark Complete until required checks and acceptance evidence are recorded. A verification task completes its stated evidence deliverable rather than claiming application code was implemented.

## Authoritative requirements

- [Layer 1 specification](../../layer_1_zone_discovery.md) — sections 6, 7, 8, 12.
- [Layer 2 specification](../../layer_2_price_reactions.md) — sections 2, 12.

The linked frozen specifications take precedence over this task summary. Do not silently introduce new strategy rules. Record any material unresolved input or implementation decision below before dependent work proceeds.

## Dependencies

- [ ] [L1-03 — Immutable zone versions and causal lineage](../layer_1/L1-03-zone-versions-and-level-history.md) completed, or an explicitly documented interface prerequisite is available.
- [ ] [F03 — Source adapters, calendar and completed candles](../foundation/F03-source-adapters-and-candle-normalization.md) completed, or an explicitly documented interface prerequisite is available.

Dependencies describe prerequisites, not permission to change another task's scope. Tests may use agreed fixtures/interfaces before full integration; acceptance still requires the actual dependencies and relevant integration checks.

## Implementation / deliverable checklist

- [ ] Implement five-minute refresh scheduling inputs, 09:30 observation start, session phase and incomplete-startup metadata.
- [ ] Separate quote freshness at valuation from snapshot TTL <=360 seconds at decision; persist newer failed-refresh health.
- [ ] Require zone availability by the tested candle's opening and retain frozen geometry separately from current health.
- [ ] Expose typed Layer 2 handoff evidence without changing geometry, replaying old alerts as fresh or creating setups on unavailable data.

## Required tests and verification

- [ ] Use the 10:05:18 availability example: first full 1m candle starts 10:06 and full 5m starts 10:10.
- [ ] Test exact 360 seconds versus greater age, newer incomplete refresh and a late startup with no morning snapshots.
- [ ] Verify an active geometry can remain observable while initiation/confirmation is blocked; morning phase never creates eligible entries.

Run focused tests for this task and relevant integration/regression checks. Record actual commands, versions and results; an unexecuted test plan is not a passing result. Follow applicable repository testing and change-process instructions during implementation.

## Assigned frozen acceptance cases

- [ ] **L1-AC06** — A 10:05:18 snapshot cannot influence a 10:05 decision or claim the earlier portion of a forming candle. ([source](../../layer_1_zone_discovery.md)).
- [ ] **L1-AC07** — TTL allows exactly 360 seconds and blocks greater ages; an incomplete refresh cannot silently fall back to healthy status. ([source](../../layer_1_zone_discovery.md)).
- [ ] **L2-AC16** — Current health uses a current snapshot, while geometry/ATR remain frozen; unavailable historical inputs cannot be borrowed from the future. ([source](../../layer_2_price_reactions.md)).

Shared cases require evidence from all listed owners in the traceability checklist. Do not close a shared acceptance case based only on this task's portion.

## Deliverables

As-of zone eligibility/phase interface and health-state tests.

Implement only within the new `spx-gex-agent` application unless a separate instruction authorizes otherwise. Record actual implementation/test paths when work begins; this plan does not prescribe unreviewed module names.

## Scope limits

No implicit 10:00 setup promotion or deadline/geometry refresh.

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

