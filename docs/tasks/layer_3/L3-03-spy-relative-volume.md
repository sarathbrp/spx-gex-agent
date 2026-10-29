# L3-03 — SPY rolling and time-of-day RVOL

**Status:** NOT_STARTED  
**Group:** layer_3  
**Task type:** Implementation  
**Owner:** Unassigned  
**Created:** 2026-10-29  
**Specification baseline:** Frozen v1.0

[Master checklist](../../TASKS.md) · [Requirements traceability](../TRACEABILITY.md)

## Objective

Measure participation with correct warm-up, historical matching and missing-data semantics.

## Completion checklist

- [ ] Implementation or deliverable completed
- [ ] Tests / verification checks passed
- [ ] Acceptance criteria verified
- [ ] Reviewed and evidence recorded
- [ ] Complete — master index updated

Every box starts unchecked. Creating this plan is not implementation or testing. Update this task and its master-index entry together; do not mark Complete until required checks and acceptance evidence are recorded. A verification task completes its stated evidence deliverable rather than claiming application code was implemented.

## Authoritative requirements

- [Layer 3 specification](../../layer_3_environmental_context.md) — sections 9, 10, 11.

The linked frozen specifications take precedence over this task summary. Do not silently introduce new strategy rules. Record any material unresolved input or implementation decision below before dependent work proceeds.

## Dependencies

- [ ] [F03 — Source adapters, calendar and completed candles](../foundation/F03-source-adapters-and-candle-normalization.md) completed, or an explicitly documented interface prerequisite is available.

Dependencies describe prerequisites, not permission to change another task's scope. Tests may use agreed fixtures/interfaces before full integration; acceptance still requires the actual dependencies and relevant integration checks.

## Implementation / deliverable checklist

- [ ] Implement 20-bar rolling RVOL using prior complete same-session bars, excluding current volume.
- [ ] Implement the TOD median over the fixed prior 10 trading sessions with at least 8 valid matching intervals and no older-session backfill.
- [ ] Retain source convention, sample dates/counts, zeros, missing intervals and early-close/DST handling.
- [ ] Keep rolling and TOD statuses independent; current-day or incompatible volume conventions cannot contaminate references.

## Required tests and verification

- [ ] Verify earliest complete 09:51/11:15 RVOL and nine prior 5m denominator bars at 10:20.
- [ ] Check genuine zero numerator, zero denominator, missing current/previous bars and denominator exclusion.
- [ ] Test 8 versus 7 TOD samples, even-count median, early-close morning/afternoon matches and no eleventh-session substitution.

Run focused tests for this task and relevant integration/regression checks. Record actual commands, versions and results; an unexecuted test plan is not a passing result. Follow applicable repository testing and change-process instructions during implementation.

## Assigned frozen acceptance cases

- [ ] **L3-AC11** — Rolling RVOL first completes at 09:51 on 1m and 11:15 on 5m; current volume is excluded from its 20-bar denominator. ([source](../../layer_3_environmental_context.md)).
- [ ] **L3-AC12** — A zero numerator can yield valid zero RVOL; a zero denominator cannot. Missing bars are not zero-volume bars. ([source](../../layer_3_environmental_context.md)).
- [ ] **L3-AC13** — TOD uses the fixed prior-ten-session list: eight valid matches pass, seven fail, and no eleventh-session backfill occurs. ([source](../../layer_3_environmental_context.md)).
- [ ] **L3-AC14** — An early-close session can supply a matching morning interval but not a post-close interval. Current-day volume never enters historical TOD references. ([source](../../layer_3_environmental_context.md)).

Shared cases require evidence from all listed owners in the traceability checklist. Do not close a shared acceptance case based only on this task's portion.

## Deliverables

SPY volume evidence calculators and calendar/missingness fixtures.

Implement only within the new `spx-gex-agent` application unless a separate instruction authorizes otherwise. Record actual implementation/test paths when work begins; this plan does not prescribe unreviewed module names.

## Scope limits

No aggressor/capital-flow claim, RVOL gate, winsorization or unapproved threshold score.

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

