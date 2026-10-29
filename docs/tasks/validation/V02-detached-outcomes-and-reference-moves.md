# V02 — Detached outcomes and descriptive reference moves

**Status:** NOT_STARTED  
**Group:** validation  
**Task type:** Implementation  
**Owner:** Unassigned  
**Created:** 2026-10-29  
**Specification baseline:** Frozen v1.0

[Master checklist](../../TASKS.md) · [Requirements traceability](../TRACEABILITY.md)

## Objective

Evaluate future paths without allowing them into original decision inputs.

## Completion checklist

- [ ] Implementation or deliverable completed
- [ ] Tests / verification checks passed
- [ ] Acceptance criteria verified
- [ ] Reviewed and evidence recorded
- [ ] Complete — master index updated

Every box starts unchecked. Creating this plan is not implementation or testing. Update this task and its master-index entry together; do not mark Complete until required checks and acceptance evidence are recorded. A verification task completes its stated evidence deliverable rather than claiming application code was implemented.

## Authoritative requirements

- [Layer 2 specification](../../layer_2_price_reactions.md) — sections 14.
- [Layer 3 specification](../../layer_3_environmental_context.md) — sections 13.
- [Layer 4 specification](../../layer_4_multitimeframe_alignment.md) — sections 10.
- [Persistence specification](../../persistent_state_and_storage.md) — sections 3, 8.

The linked frozen specifications take precedence over this task summary. Do not silently introduce new strategy rules. Record any material unresolved input or implementation decision below before dependent work proceeds.

## Dependencies

- [ ] [L2-05 — Candidate eligibility, phase gates and durable publication](../layer_2/L2-05-candidate-eligibility-and-publication.md) completed, or an explicitly documented interface prerequisite is available.
- [ ] [L4-03 — Subsequent MTF events and follow-up deadlines](../layer_4/L4-03-subsequent-comparison-events.md) completed, or an explicitly documented interface prerequisite is available.
- [ ] [P04 — Zone, state, candidate and evaluation schema](../storage/P04-domain-and-evaluation-schema.md) completed, or an explicitly documented interface prerequisite is available.

Dependencies describe prerequisites, not permission to change another task's scope. Tests may use agreed fixtures/interfaces before full integration; acceptance still requires the actual dependencies and relevant integration checks.

## Implementation / deliverable checklist

- [ ] Calculate separate next-bar extreme and next-bar close confirmation outcomes with strict comparisons and unavailable states.
- [ ] Compute 3/5/15 expected-bar SPX-point MFE/MAE from the declared reference, excluding the earlier signal-bar path.
- [ ] Calculate direction-adjusted SPX reference movement for later comparisons with timestamps/definitions; label it as descriptive, not execution slippage.
- [ ] Store results against concrete pattern/candidate/event subjects and evaluation policy; retain blocked/missing-context observations and incomplete horizons.

## Required tests and verification

- [ ] Verify missing next bars, equality cases, session-end truncation and uncompressed gap windows.
- [ ] Check CALL and PUT sign symmetry and no earlier t+1 open as a confirmed delayed-entry fill.
- [ ] Attempt outcome access through entry/context interfaces and verify isolation; check result revision/idempotency.

Run focused tests for this task and relevant integration/regression checks. Record actual commands, versions and results; an unexecuted test plan is not a passing result. Follow applicable repository testing and change-process instructions during implementation.

## Assigned frozen acceptance cases

- [ ] **L1-AC12** — Sub-threshold clusters are retained; independent 1m/5m evaluation cannot access future outcomes. ([source](../../layer_1_zone_discovery.md)).
- [ ] **L2-AC18** — Next-bar confirmations remain detached; delayed comparisons cannot enter at the earlier t+1 open or infer option fills. ([source](../../layer_2_price_reactions.md)).
- [ ] **L4-AC13** — CALL upward and PUT downward reference moves have the same positive direction-adjusted sign; neither is labeled an execution fill or slippage. ([source](../../layer_4_multitimeframe_alignment.md)).
- [ ] **L4-AC14** — Waiting policies cannot use pre-confirmation opens, later-selected best events or future outcomes in decision matching. ([source](../../layer_4_multitimeframe_alignment.md)).

Shared cases require evidence from all listed owners in the traceability checklist. Do not close a shared acceptance case based only on this task's portion.

## Deliverables

Separate evaluation workflow, reproducible metrics and underlying-point reports.

Implement only within the new `spx-gex-agent` application unless a separate instruction authorizes otherwise. Record actual implementation/test paths when work begins; this plan does not prescribe unreviewed module names.

## Scope limits

No profit claims, optimized thresholds, actual option P&L or unapproved waiting/execution policy.

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

