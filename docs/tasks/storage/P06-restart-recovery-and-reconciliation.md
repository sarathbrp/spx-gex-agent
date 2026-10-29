# P06 — Restart recovery and state reconciliation

**Status:** NOT_STARTED  
**Group:** storage  
**Task type:** Implementation  
**Owner:** Unassigned  
**Created:** 2026-10-29  
**Specification baseline:** Frozen v1.0

[Master checklist](../../TASKS.md) · [Requirements traceability](../TRACEABILITY.md)

## Objective

Resume the same session without resetting history or reviving expired opportunities.

## Completion checklist

- [ ] Implementation or deliverable completed
- [ ] Tests / verification checks passed
- [ ] Acceptance criteria verified
- [ ] Reviewed and evidence recorded
- [ ] Complete — master index updated

Every box starts unchecked. Creating this plan is not implementation or testing. Update this task and its master-index entry together; do not mark Complete until required checks and acceptance evidence are recorded. A verification task completes its stated evidence deliverable rather than claiming application code was implemented.

## Authoritative requirements

- [Layer 1 specification](../../layer_1_zone_discovery.md) — sections 8, 9, 11.
- [Layer 2 specification](../../layer_2_price_reactions.md) — sections 5, 9, 11, 12.
- [Layer 3 specification](../../layer_3_environmental_context.md) — sections 13.
- [Layer 4 specification](../../layer_4_multitimeframe_alignment.md) — sections 9, 12.
- [Persistence specification](../../persistent_state_and_storage.md) — sections 9, 12, 13.

The linked frozen specifications take precedence over this task summary. Do not silently introduce new strategy rules. Record any material unresolved input or implementation decision below before dependent work proceeds.

## Dependencies

- [ ] [I01 — Live engine orchestration and session scheduling](../integration/I01-live-engine-orchestration.md) completed, or an explicitly documented interface prerequisite is available.

Dependencies describe prerequisites, not permission to change another task's scope. Tests may use agreed fixtures/interfaces before full integration; acceptance still requires the actual dependencies and relevant integration checks.

## Implementation / deliverable checklist

- [ ] Restore compatible projections/checkpoints and rebuild inconsistent projections from recorded events.
- [ ] Process committed tails in original availability order, retaining run-versus-stream identity and historical/recovery output labels.
- [ ] Reconcile required price gaps, GEX-health failures, session rollover and original watch/MTF deadlines before fresh evaluation.
- [ ] Keep late provider backfills/revisions retrospective or audited; do not erase morning gaps or reannounce blocked historical triggers as fresh.

## Required tests and verification

- [ ] Restart mid-test, during re-arm, pending retest, following candidate commit and during MTF follow-up.
- [ ] Compare restored geometry, ATR, indices, missingness and deadlines with uninterrupted execution.
- [ ] Corrupt a rebuildable projection and verify journal-based recovery; simulate downtime gaps and session close.

Run focused tests for this task and relevant integration/regression checks. Record actual commands, versions and results; an unexecuted test plan is not a passing result. Follow applicable repository testing and change-process instructions during implementation.

## Assigned frozen acceptance cases

- [ ] **L1-AC10** — Restart and repeated ingestion produce the same event identities and no duplicate counts. ([source](../../layer_1_zone_discovery.md)).
- [ ] **L2-AC14** — Each watch has at most one candidate; blocked triggers are not queued; consumed records survive restart. ([source](../../layer_2_price_reactions.md)).
- [ ] **L3-AC15** — Repeated processing and restart preserve original context, missingness and candidate identity; later outcomes cannot alter evidence. ([source](../../layer_3_environmental_context.md)).
- [ ] **L4-AC15** — Restart/replay preserve candidate IDs, comparison cutoffs, audit deduplication and deadlines without re-emission. ([source](../../layer_4_multitimeframe_alignment.md)).
- [ ] **P-AC06** — Frozen geometry, ATR, test scope and original deadlines survive restart; history counts agree with journal reconstruction. ([source](../../persistent_state_and_storage.md)).

Shared cases require evidence from all listed owners in the traceability checklist. Do not close a shared acceptance case based only on this task's portion.

## Deliverables

Recovery workflow, reconciliation diagnostics and restart equivalence tests.

Implement only within the new `spx-gex-agent` application unless a separate instruction authorizes otherwise. Record actual implementation/test paths when work begins; this plan does not prescribe unreviewed module names.

## Scope limits

No guessed counter repairs, refreshed deadlines or automatic recovery entries.

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

