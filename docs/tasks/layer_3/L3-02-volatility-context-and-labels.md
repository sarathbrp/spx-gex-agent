# L3-02 — VIX/VIX1D changes, slopes and descriptive labels

**Status:** NOT_STARTED  
**Group:** layer_3  
**Task type:** Implementation  
**Owner:** Unassigned  
**Created:** 2026-10-29  
**Specification baseline:** Frozen v1.0

[Master checklist](../../TASKS.md) · [Requirements traceability](../TRACEABILITY.md)

## Objective

Publish aligned volatility evidence with separate availability and research-only direction labels.

## Completion checklist

- [ ] Implementation or deliverable completed
- [ ] Tests / verification checks passed
- [ ] Acceptance criteria verified
- [ ] Reviewed and evidence recorded
- [ ] Complete — master index updated

Every box starts unchecked. Creating this plan is not implementation or testing. Update this task and its master-index entry together; do not mark Complete until required checks and acceptance evidence are recorded. A verification task completes its stated evidence deliverable rather than claiming application code was implemented.

## Authoritative requirements

- [Layer 3 specification](../../layer_3_environmental_context.md) — sections 6, 7, 8.

The linked frozen specifications take precedence over this task summary. Do not silently introduce new strategy rules. Record any material unresolved input or implementation decision below before dependent work proceeds.

## Dependencies

- [ ] [F03 — Source adapters, calendar and completed candles](../foundation/F03-source-adapters-and-candle-normalization.md) completed, or an explicitly documented interface prerequisite is available.

Dependencies describe prerequisites, not permission to change another task's scope. Tests may use agreed fixtures/interfaces before full integration; acceptance still requires the actual dependencies and relevant integration checks.

## Implementation / deliverable checklist

- [ ] Require exact candidate-aligned completed reference bars and same-session 15-minute endpoints.
- [ ] Compute five-consecutive-close OLS slopes in points per minute with 4/20-minute elapsed spans.
- [ ] Implement VIX1D CALL/PUT thresholds, strict slope signs and exhaustive inconclusive/unavailable handling.
- [ ] Keep VIX and VIX1D statuses independent; never substitute missing VIX1D or use a later bar.

## Required tests and verification

- [ ] Test exact timestamps, internal gaps, placeholder bars and same-session boundaries.
- [ ] Verify -0.15/+0.25 endpoints, zero/contrary slopes and mirrored candidate sides.
- [ ] Check normalization on equivalent linear inputs and missing VIX with valid VIX1D classification.

Run focused tests for this task and relevant integration/regression checks. Record actual commands, versions and results; an unexecuted test plan is not a passing result. Follow applicable repository testing and change-process instructions during implementation.

## Assigned frozen acceptance cases

- [ ] **L3-AC08** — Exact 15-minute endpoints and five consecutive slope observations are required; 1m/5m slopes use 4/20-minute elapsed spans and point-per-minute units. ([source](../../layer_3_environmental_context.md)).
- [ ] **L3-AC09** — CALL delta -0.15 with negative slope is supportive; PUT delta +0.25 with positive slope is supportive. Zero or contrary slope is inconclusive; missing required input is unavailable. ([source](../../layer_3_environmental_context.md)).
- [ ] **L3-AC10** — Missing VIX does not erase valid VIX1D classification. Classification never vetoes a candidate. ([source](../../layer_3_environmental_context.md)).

Shared cases require evidence from all listed owners in the traceability checklist. Do not close a shared acceptance case based only on this task's portion.

## Deliverables

Volatility evidence evaluator and alignment/classification fixtures.

Implement only within the new `spx-gex-agent` application unless a separate instruction authorizes otherwise. Record actual implementation/test paths when work begins; this plan does not prescribe unreviewed module names.

## Scope limits

No entry veto or inference of actual dealer hedging or SPX path.

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

