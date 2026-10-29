# L3-04 — Immutable environmental context and late enrichment

**Status:** NOT_STARTED  
**Group:** layer_3  
**Task type:** Implementation  
**Owner:** Unassigned  
**Created:** 2026-10-29  
**Specification baseline:** Frozen v1.0

[Master checklist](../../TASKS.md) · [Requirements traceability](../TRACEABILITY.md)

## Objective

Attach causal partial context without delaying or revising the Layer 2 baseline.

## Completion checklist

- [ ] Implementation or deliverable completed
- [ ] Tests / verification checks passed
- [ ] Acceptance criteria verified
- [ ] Reviewed and evidence recorded
- [ ] Complete — master index updated

Every box starts unchecked. Creating this plan is not implementation or testing. Update this task and its master-index entry together; do not mark Complete until required checks and acceptance evidence are recorded. A verification task completes its stated evidence deliverable rather than claiming application code was implemented.

## Authoritative requirements

- [Layer 3 specification](../../layer_3_environmental_context.md) — sections 1, 2, 12, 13.
- [Persistence specification](../../persistent_state_and_storage.md) — sections 8.

The linked frozen specifications take precedence over this task summary. Do not silently introduce new strategy rules. Record any material unresolved input or implementation decision below before dependent work proceeds.

## Dependencies

- [ ] [L3-01 — Broad GEX and synchronized ex-zone context](../layer_3/L3-01-broad-and-ex-zone-gex.md) completed, or an explicitly documented interface prerequisite is available.
- [ ] [L3-02 — VIX/VIX1D changes, slopes and descriptive labels](../layer_3/L3-02-volatility-context-and-labels.md) completed, or an explicitly documented interface prerequisite is available.
- [ ] [L3-03 — SPY rolling and time-of-day RVOL](../layer_3/L3-03-spy-relative-volume.md) completed, or an explicitly documented interface prerequisite is available.
- [ ] [L2-05 — Candidate eligibility, phase gates and durable publication](../layer_2/L2-05-candidate-eligibility-and-publication.md) completed, or an explicitly documented interface prerequisite is available.
- [ ] [P05 — Atomic transitions, projections and checkpoints](../storage/P05-atomic-transitions-and-checkpoints.md) completed, or an explicitly documented interface prerequisite is available.

Dependencies describe prerequisites, not permission to change another task's scope. Tests may use agreed fixtures/interfaces before full integration; acceptance still requires the actual dependencies and relevant integration checks.

## Implementation / deliverable checklist

- [ ] Build original context from the recorded candidate cutoff and independent metric availability.
- [ ] Store absent factors explicitly while preserving valid factors and original configuration/source references.
- [ ] Route later inputs and corrections to append-only enrichment audit; preserve original missingness.
- [ ] Keep enrichment/outcome consumers separate from required candidate transitions and exclude future outcomes from context reads.

## Required tests and verification

- [ ] Emit a valid candidate with missing broadGEX, VIX1D and pre-warmup RVOL and confirm it survives unchanged.
- [ ] Test late same-close inputs, recomputation after arrival and restart idempotency without replacing original context.
- [ ] Verify context failures do not roll back candidates and evaluation population retains missing-context rows.

Run focused tests for this task and relevant integration/regression checks. Record actual commands, versions and results; an unexecuted test plan is not a passing result. Follow applicable repository testing and change-process instructions during implementation.

## Assigned frozen acceptance cases

- [ ] **L3-AC01** — An eligible 10:20 5m candidate survives unavailable rolling RVOL, absent VIX1D and incomplete broad GEX; each factor carries its own reason. ([source](../../layer_3_environmental_context.md)).
- [ ] **L3-AC07** — A late snapshot or late VIX/SPY bar only appears in enrichment audit. Later computation cannot move the original input cutoff. ([source](../../layer_3_environmental_context.md)).
- [ ] **L3-AC15** — Repeated processing and restart preserve original context, missingness and candidate identity; later outcomes cannot alter evidence. ([source](../../layer_3_environmental_context.md)).
- [ ] **P-AC09** — Price-data gaps invalidate sequences; optional context outages do not remove eligible Layer 2 candidates. ([source](../../persistent_state_and_storage.md)).

Shared cases require evidence from all listed owners in the traceability checklist. Do not close a shared acceptance case based only on this task's portion.

## Deliverables

Layer 3 consumer, immutable evidence records and optional-failure integration tests.

Implement only within the new `spx-gex-agent` application unless a separate instruction authorizes otherwise. Record actual implementation/test paths when work begins; this plan does not prescribe unreviewed module names.

## Scope limits

No score points, delayed candidate emission or silent selection of complete-context candidates only.

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

