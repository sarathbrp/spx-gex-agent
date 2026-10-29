# L2-05 — Candidate eligibility, phase gates and durable publication

**Status:** NOT_STARTED  
**Group:** layer_2  
**Task type:** Implementation  
**Owner:** Unassigned  
**Created:** 2026-10-29  
**Specification baseline:** Frozen v1.0

[Master checklist](../../TASKS.md) · [Requirements traceability](../TRACEABILITY.md)

## Objective

Separate detected patterns from eligible candidates and commit their full decision-time evidence.

## Completion checklist

- [ ] Implementation or deliverable completed
- [ ] Tests / verification checks passed
- [ ] Acceptance criteria verified
- [ ] Reviewed and evidence recorded
- [ ] Complete — master index updated

Every box starts unchecked. Creating this plan is not implementation or testing. Update this task and its master-index entry together; do not mark Complete until required checks and acceptance evidence are recorded. A verification task completes its stated evidence deliverable rather than claiming application code was implemented.

## Authoritative requirements

- [Layer 2 specification](../../layer_2_price_reactions.md) — sections 1, 12, 13.
- [Layer 1 specification](../../layer_1_zone_discovery.md) — sections 8.
- [Persistence specification](../../persistent_state_and_storage.md) — sections 7, 10, 11.

The linked frozen specifications take precedence over this task summary. Do not silently introduce new strategy rules. Record any material unresolved input or implementation decision below before dependent work proceeds.

## Dependencies

- [ ] [L2-03 — Rejection geometry and traversal observations](../layer_2/L2-03-rejection-and-traversal-patterns.md) completed, or an explicitly documented interface prerequisite is available.
- [ ] [L2-04 — Fresh breakout and retest watch lifecycle](../layer_2/L2-04-breakout-and-retest-watches.md) completed, or an explicitly documented interface prerequisite is available.
- [ ] [P07 — Durable outbox and display deduplication](../storage/P07-durable-outbox-and-display-deduplication.md) completed, or an explicitly documented interface prerequisite is available.

Dependencies describe prerequisites, not permission to change another task's scope. Tests may use agreed fixtures/interfaces before full integration; acceptance still requires the actual dependencies and relevant integration checks.

## Implementation / deliverable checklist

- [ ] Apply required current Layer 1 health, complete price evidence, frozen ATR and session-phase checks.
- [ ] Require confirmation candles to start at/after 10:00 and end before session close; preserve morning patterns without automatic promotion.
- [ ] Commit pattern, blockers or eligible candidate, terminal transition and delivery intent atomically.
- [ ] Persist geometric proof, source times/cursor, exact test scope, frozen/current context distinction and all eligibility reasons.

## Required tests and verification

- [ ] Verify earliest 10:01/10:05 confirmations, exact-close exclusion and fresh post-10:00 confirmation for an existing morning setup.
- [ ] Check GEX failure versus price gap, blocked trigger consumption and no recovery release.
- [ ] Inject optional Layer 3/4 failures and assert unchanged candidate population and immutable evidence.

Run focused tests for this task and relevant integration/regression checks. Record actual commands, versions and results; an unexecuted test plan is not a passing result. Follow applicable repository testing and change-process instructions during implementation.

## Assigned frozen acceptance cases

- [ ] **L1-AC08** — A blocked trigger is not released automatically after recovery. ([source](../../layer_1_zone_discovery.md)).
- [ ] **L2-AC14** — Each watch has at most one candidate; blocked triggers are not queued; consumed records survive restart. ([source](../../layer_2_price_reactions.md)).
- [ ] **L2-AC15** — Morning observations persist, but candidates require a fresh confirmation candle starting at or after 10:00. ([source](../../layer_2_price_reactions.md)).
- [ ] **L2-AC16** — Current health uses a current snapshot, while geometry/ATR remain frozen; unavailable historical inputs cannot be borrowed from the future. ([source](../../layer_2_price_reactions.md)).
- [ ] **P-AC09** — Price-data gaps invalidate sequences; optional context outages do not remove eligible Layer 2 candidates. ([source](../../persistent_state_and_storage.md)).

Shared cases require evidence from all listed owners in the traceability checklist. Do not close a shared acceptance case based only on this task's portion.

## Deliverables

Layer 2 decision workflow and candidate publication integration tests.

Implement only within the new `spx-gex-agent` application unless a separate instruction authorizes otherwise. Record actual implementation/test paths when work begins; this plan does not prescribe unreviewed module names.

## Scope limits

No scoring, position management, option contract selection or real/simulated fill inference.

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

