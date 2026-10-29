# I01 — Live engine orchestration and session scheduling

**Status:** NOT_STARTED  
**Group:** integration  
**Task type:** Implementation  
**Owner:** Unassigned  
**Created:** 2026-10-29  
**Specification baseline:** Frozen v1.0

[Master checklist](../../TASKS.md) · [Requirements traceability](../TRACEABILITY.md)

## Objective

Connect the implemented layers in one deterministic writer process without making optional context a candidate prerequisite.

## Completion checklist

- [ ] Implementation or deliverable completed
- [ ] Tests / verification checks passed
- [ ] Acceptance criteria verified
- [ ] Reviewed and evidence recorded
- [ ] Complete — master index updated

Every box starts unchecked. Creating this plan is not implementation or testing. Update this task and its master-index entry together; do not mark Complete until required checks and acceptance evidence are recorded. A verification task completes its stated evidence deliverable rather than claiming application code was implemented.

## Authoritative requirements

- [Layer 1 specification](../../layer_1_zone_discovery.md) — sections 6, 7, 8.
- [Layer 2 specification](../../layer_2_price_reactions.md) — sections 12.
- [Layer 3 specification](../../layer_3_environmental_context.md) — sections 1, 2.
- [Layer 4 specification](../../layer_4_multitimeframe_alignment.md) — sections 1, 3.
- [Persistence specification](../../persistent_state_and_storage.md) — sections 2, 9, 10, 12.

The linked frozen specifications take precedence over this task summary. Do not silently introduce new strategy rules. Record any material unresolved input or implementation decision below before dependent work proceeds.

## Dependencies

- [ ] [F03 — Source adapters, calendar and completed candles](../foundation/F03-source-adapters-and-candle-normalization.md) completed, or an explicitly documented interface prerequisite is available.
- [ ] [P03 — Single-writer ownership and read-only access](../storage/P03-single-writer-and-readonly-viewers.md) completed, or an explicitly documented interface prerequisite is available.
- [ ] [P07 — Durable outbox and display deduplication](../storage/P07-durable-outbox-and-display-deduplication.md) completed, or an explicitly documented interface prerequisite is available.
- [ ] [L1-04 — Snapshot eligibility, phases and zone handoff](../layer_1/L1-04-snapshot-eligibility-and-session-observation.md) completed, or an explicitly documented interface prerequisite is available.
- [ ] [L2-05 — Candidate eligibility, phase gates and durable publication](../layer_2/L2-05-candidate-eligibility-and-publication.md) completed, or an explicitly documented interface prerequisite is available.
- [ ] [L3-04 — Immutable environmental context and late enrichment](../layer_3/L3-04-immutable-environment-and-late-audit.md) completed, or an explicitly documented interface prerequisite is available.
- [ ] [L4-03 — Subsequent MTF events and follow-up deadlines](../layer_4/L4-03-subsequent-comparison-events.md) completed, or an explicitly documented interface prerequisite is available.

Dependencies describe prerequisites, not permission to change another task's scope. Tests may use agreed fixtures/interfaces before full integration; acceptance still requires the actual dependencies and relevant integration checks.

## Implementation / deliverable checklist

- [ ] Schedule source collection, five-minute GEX refreshes and completed-bar evaluation under exclusive writer ownership.
- [ ] Run separate 1m/5m Layer 2 state with shared causal source inputs, fixed event ordering and durable deadline events.
- [ ] Publish committed Layer 2 results promptly; consume Layer 3/4 work independently against recorded cutoffs.
- [ ] Expose start/stop, phase, health, config identity and operational errors; integrate recover-before-run through the P06 workflow once available.

## Required tests and verification

- [ ] Run a recorded session fixture through collection, zones, both timeframes, candidates and optional context.
- [ ] Inject slow/failed optional requests and verify unaffected eligible candidate emission and recorded unavailable factors.
- [ ] Check 09:30/10:00/session-close scheduling, graceful shutdown and second-writer rejection.

Run focused tests for this task and relevant integration/regression checks. Record actual commands, versions and results; an unexecuted test plan is not a passing result. Follow applicable repository testing and change-process instructions during implementation.

## Assigned frozen acceptance cases

- [ ] **L2-AC17** — The 1m and 5m paths remain independent; shared-boundary observations do not generate duplicate ownership events. ([source](../../layer_2_price_reactions.md)).
- [ ] **L4-AC01** — Every eligible Layer 2 candidate survives all five comparison states and all Layer 4 errors. ([source](../../layer_4_multitimeframe_alignment.md)).

Shared cases require evidence from all listed owners in the traceability checklist. Do not close a shared acceptance case based only on this task's portion.

## Deliverables

Minimal engine entry point/orchestrator and integration fixtures; no specific UI layout assumed.

Implement only within the new `spx-gex-agent` application unless a separate instruction authorizes otherwise. Record actual implementation/test paths when work begins; this plan does not prescribe unreviewed module names.

## Scope limits

No new strategy rules, score formula, broker/paper execution or imported ORB gates.

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

