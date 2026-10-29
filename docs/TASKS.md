# Implementation Task Checklist

**Project:** spx-gex-agent  
**Created:** 2026-10-29  
**Baseline:** Layers 1–4 and persistence, frozen v1.0  
**Initial progress:** 0 of 33 tasks complete; no implementation or test result is implied by these documents.

This is the master implementation/verification checklist. Each task has its own Markdown file, dependencies, scoped requirements, test cases, acceptance ownership and evidence fields. [Requirements traceability](tasks/TRACEABILITY.md) maps all 67 numbered specification sections and 72 frozen acceptance cases to tasks.

## Status and completion rules

- Task status values: NOT_STARTED, IN_PROGRESS, BLOCKED, IN_REVIEW, COMPLETE. All tasks initially use NOT_STARTED.
- Implementation/deliverable complete means the stated changes or verification artifact exist; it does not mean tested.
- Tests/checks passed requires actual recorded results. Provider/audit tasks may use documented probes and inspection; algorithm/storage tasks require the automated checks specified in their files.
- Acceptance verified means task criteria and its part of every assigned frozen acceptance case are supported by evidence. Shared cases close only after all owners have evidence.
- Reviewed means the result was checked against the frozen contract, with review evidence and unresolved findings recorded. It does not require spawning another agent.
- Complete requires the preceding stages, resolved material blockers and an updated evidence/handoff section. Update task status, its checklist, this index and any completed traceability cases together.
- Keep live observation, automated fixture validation and research performance claims separate. Unobserved live behavior stays unverified; no market signal should be fabricated to close a checklist.
- The summary counts below are initial/manual tracking counts; maintain them when tasks complete. Do not mark tasks done merely because their task files were created.

## Scope and authority

Implement the frozen research observation application in the separate spx-gex-agent folder. Existing spx_orb code/data remain unchanged. Layers 3 and 4 are observation-only and cannot suppress eligible Layer 2 candidates. SQLite has one engine writer and multiple read-only viewers.

Combined score calibration, profitability validation, paper/broker execution, portfolio management and detailed TUI visual design are not authorized by this task breakdown. Minimal read-only monitoring is included. Provider capabilities and unresolved valuation details must be verified in F02 rather than assumed from the design.

Source specifications:

- [Layer 1](layer_1_zone_discovery.md)
- [Layer 2](layer_2_price_reactions.md)
- [Layer 3](layer_3_environmental_context.md)
- [Layer 4](layer_4_multitimeframe_alignment.md)
- [Persistence](persistent_state_and_storage.md)

The frozen specifications take precedence over task summaries. If implementation exposes a genuine contract ambiguity, record it explicitly and resolve it before dependent code; do not reinterpret the strategy inside a task.

## Task summary

| Group | Tasks | Complete |
|---|---:|---:|
| Shared foundation | 3 | 0 |
| Persistence and storage | 8 | 0 |
| Layer 1 — Zone discovery | 4 | 0 |
| Layer 2 — Price reactions | 5 | 0 |
| Layer 3 — Environmental context | 4 | 0 |
| Layer 4 — Multi-timeframe alignment | 3 | 0 |
| Runtime integration and monitoring | 2 | 0 |
| Replay, evaluation and acceptance | 4 | 0 |
| **Total** | **33** | **0** |

## Suggested dependency-respecting order

This is one valid order, not a requirement to finish every storage task before any algorithm work. Dependency lists in each task are authoritative. In particular, P06 recovery verification follows the integrated engine, and P08 maintenance can be completed once its schema/ownership prerequisites exist.

1. [F01 — Project contracts, configuration and test harness](tasks/foundation/F01-project-contracts-and-test-harness.md)
2. [F02 — Provider capabilities and reusable-code audit](tasks/foundation/F02-provider-and-reuse-verification.md)
3. [P01 — SQLite bootstrap, metadata and source schema](tasks/storage/P01-sqlite-bootstrap-and-source-schema.md)
4. [P02 — Journal ingestion, publication and causal reads](tasks/storage/P02-journal-ingestion-and-asof-reads.md)
5. [F03 — Source adapters, calendar and completed candles](tasks/foundation/F03-source-adapters-and-candle-normalization.md)
6. [P03 — Single-writer ownership and read-only access](tasks/storage/P03-single-writer-and-readonly-viewers.md)
7. [P04 — Zone, state, candidate and evaluation schema](tasks/storage/P04-domain-and-evaluation-schema.md)
8. [P05 — Atomic transitions, projections and checkpoints](tasks/storage/P05-atomic-transitions-and-checkpoints.md)
9. [P07 — Durable outbox and display deduplication](tasks/storage/P07-durable-outbox-and-display-deduplication.md)
10. [L1-01 — OI universe, contract valuation and local coverage](tasks/layer_1/L1-01-oi-universe-and-gex-validation.md)
11. [L1-02 — Deterministic clusters, dominance and composition](tasks/layer_1/L1-02-clustering-dominance-and-composition.md)
12. [L1-03 — Immutable zone versions and causal lineage](tasks/layer_1/L1-03-zone-versions-and-level-history.md)
13. [L1-04 — Snapshot eligibility, phases and zone handoff](tasks/layer_1/L1-04-snapshot-eligibility-and-session-observation.md)
14. [L2-01 — Causal ATR warm-up and frozen buffers](tasks/layer_2/L2-01-atr-warmup-and-frozen-buffers.md)
15. [L2-02 — Interaction episodes, contact ownership and re-arming](tasks/layer_2/L2-02-episodes-contact-and-rearming.md)
16. [L2-03 — Rejection geometry and traversal observations](tasks/layer_2/L2-03-rejection-and-traversal-patterns.md)
17. [L2-04 — Fresh breakout and retest watch lifecycle](tasks/layer_2/L2-04-breakout-and-retest-watches.md)
18. [L2-05 — Candidate eligibility, phase gates and durable publication](tasks/layer_2/L2-05-candidate-eligibility-and-publication.md)
19. [L3-01 — Broad GEX and synchronized ex-zone context](tasks/layer_3/L3-01-broad-and-ex-zone-gex.md)
20. [L3-02 — VIX/VIX1D changes, slopes and descriptive labels](tasks/layer_3/L3-02-volatility-context-and-labels.md)
21. [L3-03 — SPY rolling and time-of-day RVOL](tasks/layer_3/L3-03-spy-relative-volume.md)
22. [L3-04 — Immutable environmental context and late enrichment](tasks/layer_3/L3-04-immutable-environment-and-late-audit.md)
23. [L4-01 — Spatial/timeframe matching and five-state resolution](tasks/layer_4/L4-01-matching-and-state-priority.md)
24. [L4-02 — Immutable decision-time MTF records](tasks/layer_4/L4-02-decision-time-mtf-records.md)
25. [L4-03 — Subsequent MTF events and follow-up deadlines](tasks/layer_4/L4-03-subsequent-comparison-events.md)
26. [I01 — Live engine orchestration and session scheduling](tasks/integration/I01-live-engine-orchestration.md)
27. [P06 — Restart recovery and state reconciliation](tasks/storage/P06-restart-recovery-and-reconciliation.md)
28. [P08 — Backup, migration and restore operations](tasks/storage/P08-backup-migrations-and-maintenance.md)
29. [I02 — Read-only monitoring and operator status](tasks/integration/I02-readonly-monitoring-and-operator-status.md)
30. [V01 — Deterministic recorded-input replay](tasks/validation/V01-deterministic-recorded-input-replay.md)
31. [V02 — Detached outcomes and descriptive reference moves](tasks/validation/V02-detached-outcomes-and-reference-moves.md)
32. [V03 — Cross-layer acceptance and failure validation](tasks/validation/V03-cross-layer-acceptance-and-fault-injection.md)
33. [V04 — Live observation validation and operational runbook](tasks/validation/V04-live-observation-validation-and-runbook.md)

Start with F01 and F02: the new app's contracts/test harness and evidence-backed provider/reuse audit. These tasks expose prerequisites before adapter or strategy work depends on them.

## Shared foundation

- [ ] **[F01 — Project contracts, configuration and test harness](tasks/foundation/F01-project-contracts-and-test-harness.md) — Complete**
  - [ ] Implementation or deliverable completed
  - [ ] Tests / verification checks passed
  - [ ] Acceptance criteria verified
  - [ ] Reviewed and evidence recorded
  - Dependencies: None.

- [ ] **[F02 — Provider capabilities and reusable-code audit](tasks/foundation/F02-provider-and-reuse-verification.md) — Complete**
  - [ ] Implementation or deliverable completed
  - [ ] Tests / verification checks passed
  - [ ] Acceptance criteria verified
  - [ ] Reviewed and evidence recorded
  - Dependencies: None.

- [ ] **[F03 — Source adapters, calendar and completed candles](tasks/foundation/F03-source-adapters-and-candle-normalization.md) — Complete**
  - [ ] Implementation or deliverable completed
  - [ ] Tests / verification checks passed
  - [ ] Acceptance criteria verified
  - [ ] Reviewed and evidence recorded
  - Dependencies: [F01](tasks/foundation/F01-project-contracts-and-test-harness.md), [F02](tasks/foundation/F02-provider-and-reuse-verification.md), [P02](tasks/storage/P02-journal-ingestion-and-asof-reads.md).


## Persistence and storage

- [ ] **[P01 — SQLite bootstrap, metadata and source schema](tasks/storage/P01-sqlite-bootstrap-and-source-schema.md) — Complete**
  - [ ] Implementation or deliverable completed
  - [ ] Tests / verification checks passed
  - [ ] Acceptance criteria verified
  - [ ] Reviewed and evidence recorded
  - Dependencies: [F01](tasks/foundation/F01-project-contracts-and-test-harness.md).

- [ ] **[P02 — Journal ingestion, publication and causal reads](tasks/storage/P02-journal-ingestion-and-asof-reads.md) — Complete**
  - [ ] Implementation or deliverable completed
  - [ ] Tests / verification checks passed
  - [ ] Acceptance criteria verified
  - [ ] Reviewed and evidence recorded
  - Dependencies: [P01](tasks/storage/P01-sqlite-bootstrap-and-source-schema.md).

- [ ] **[P03 — Single-writer ownership and read-only access](tasks/storage/P03-single-writer-and-readonly-viewers.md) — Complete**
  - [ ] Implementation or deliverable completed
  - [ ] Tests / verification checks passed
  - [ ] Acceptance criteria verified
  - [ ] Reviewed and evidence recorded
  - Dependencies: [P01](tasks/storage/P01-sqlite-bootstrap-and-source-schema.md).

- [ ] **[P04 — Zone, state, candidate and evaluation schema](tasks/storage/P04-domain-and-evaluation-schema.md) — Complete**
  - [ ] Implementation or deliverable completed
  - [ ] Tests / verification checks passed
  - [ ] Acceptance criteria verified
  - [ ] Reviewed and evidence recorded
  - Dependencies: [P01](tasks/storage/P01-sqlite-bootstrap-and-source-schema.md).

- [ ] **[P05 — Atomic transitions, projections and checkpoints](tasks/storage/P05-atomic-transitions-and-checkpoints.md) — Complete**
  - [ ] Implementation or deliverable completed
  - [ ] Tests / verification checks passed
  - [ ] Acceptance criteria verified
  - [ ] Reviewed and evidence recorded
  - Dependencies: [P02](tasks/storage/P02-journal-ingestion-and-asof-reads.md), [P04](tasks/storage/P04-domain-and-evaluation-schema.md).

- [ ] **[P06 — Restart recovery and state reconciliation](tasks/storage/P06-restart-recovery-and-reconciliation.md) — Complete**
  - [ ] Implementation or deliverable completed
  - [ ] Tests / verification checks passed
  - [ ] Acceptance criteria verified
  - [ ] Reviewed and evidence recorded
  - Dependencies: [I01](tasks/integration/I01-live-engine-orchestration.md).

- [ ] **[P07 — Durable outbox and display deduplication](tasks/storage/P07-durable-outbox-and-display-deduplication.md) — Complete**
  - [ ] Implementation or deliverable completed
  - [ ] Tests / verification checks passed
  - [ ] Acceptance criteria verified
  - [ ] Reviewed and evidence recorded
  - Dependencies: [P03](tasks/storage/P03-single-writer-and-readonly-viewers.md), [P05](tasks/storage/P05-atomic-transitions-and-checkpoints.md).

- [ ] **[P08 — Backup, migration and restore operations](tasks/storage/P08-backup-migrations-and-maintenance.md) — Complete**
  - [ ] Implementation or deliverable completed
  - [ ] Tests / verification checks passed
  - [ ] Acceptance criteria verified
  - [ ] Reviewed and evidence recorded
  - Dependencies: [P03](tasks/storage/P03-single-writer-and-readonly-viewers.md), [P04](tasks/storage/P04-domain-and-evaluation-schema.md).


## Layer 1 — Zone discovery

- [ ] **[L1-01 — OI universe, contract valuation and local coverage](tasks/layer_1/L1-01-oi-universe-and-gex-validation.md) — Complete**
  - [ ] Implementation or deliverable completed
  - [ ] Tests / verification checks passed
  - [ ] Acceptance criteria verified
  - [ ] Reviewed and evidence recorded
  - Dependencies: [F03](tasks/foundation/F03-source-adapters-and-candle-normalization.md).

- [ ] **[L1-02 — Deterministic clusters, dominance and composition](tasks/layer_1/L1-02-clustering-dominance-and-composition.md) — Complete**
  - [ ] Implementation or deliverable completed
  - [ ] Tests / verification checks passed
  - [ ] Acceptance criteria verified
  - [ ] Reviewed and evidence recorded
  - Dependencies: [L1-01](tasks/layer_1/L1-01-oi-universe-and-gex-validation.md).

- [ ] **[L1-03 — Immutable zone versions and causal lineage](tasks/layer_1/L1-03-zone-versions-and-level-history.md) — Complete**
  - [ ] Implementation or deliverable completed
  - [ ] Tests / verification checks passed
  - [ ] Acceptance criteria verified
  - [ ] Reviewed and evidence recorded
  - Dependencies: [L1-02](tasks/layer_1/L1-02-clustering-dominance-and-composition.md), [P05](tasks/storage/P05-atomic-transitions-and-checkpoints.md).

- [ ] **[L1-04 — Snapshot eligibility, phases and zone handoff](tasks/layer_1/L1-04-snapshot-eligibility-and-session-observation.md) — Complete**
  - [ ] Implementation or deliverable completed
  - [ ] Tests / verification checks passed
  - [ ] Acceptance criteria verified
  - [ ] Reviewed and evidence recorded
  - Dependencies: [L1-03](tasks/layer_1/L1-03-zone-versions-and-level-history.md), [F03](tasks/foundation/F03-source-adapters-and-candle-normalization.md).


## Layer 2 — Price reactions

- [ ] **[L2-01 — Causal ATR warm-up and frozen buffers](tasks/layer_2/L2-01-atr-warmup-and-frozen-buffers.md) — Complete**
  - [ ] Implementation or deliverable completed
  - [ ] Tests / verification checks passed
  - [ ] Acceptance criteria verified
  - [ ] Reviewed and evidence recorded
  - Dependencies: [F03](tasks/foundation/F03-source-adapters-and-candle-normalization.md).

- [ ] **[L2-02 — Interaction episodes, contact ownership and re-arming](tasks/layer_2/L2-02-episodes-contact-and-rearming.md) — Complete**
  - [ ] Implementation or deliverable completed
  - [ ] Tests / verification checks passed
  - [ ] Acceptance criteria verified
  - [ ] Reviewed and evidence recorded
  - Dependencies: [L1-04](tasks/layer_1/L1-04-snapshot-eligibility-and-session-observation.md), [L2-01](tasks/layer_2/L2-01-atr-warmup-and-frozen-buffers.md), [P05](tasks/storage/P05-atomic-transitions-and-checkpoints.md).

- [ ] **[L2-03 — Rejection geometry and traversal observations](tasks/layer_2/L2-03-rejection-and-traversal-patterns.md) — Complete**
  - [ ] Implementation or deliverable completed
  - [ ] Tests / verification checks passed
  - [ ] Acceptance criteria verified
  - [ ] Reviewed and evidence recorded
  - Dependencies: [L2-02](tasks/layer_2/L2-02-episodes-contact-and-rearming.md).

- [ ] **[L2-04 — Fresh breakout and retest watch lifecycle](tasks/layer_2/L2-04-breakout-and-retest-watches.md) — Complete**
  - [ ] Implementation or deliverable completed
  - [ ] Tests / verification checks passed
  - [ ] Acceptance criteria verified
  - [ ] Reviewed and evidence recorded
  - Dependencies: [L2-02](tasks/layer_2/L2-02-episodes-contact-and-rearming.md).

- [ ] **[L2-05 — Candidate eligibility, phase gates and durable publication](tasks/layer_2/L2-05-candidate-eligibility-and-publication.md) — Complete**
  - [ ] Implementation or deliverable completed
  - [ ] Tests / verification checks passed
  - [ ] Acceptance criteria verified
  - [ ] Reviewed and evidence recorded
  - Dependencies: [L2-03](tasks/layer_2/L2-03-rejection-and-traversal-patterns.md), [L2-04](tasks/layer_2/L2-04-breakout-and-retest-watches.md), [P07](tasks/storage/P07-durable-outbox-and-display-deduplication.md).


## Layer 3 — Environmental context

- [ ] **[L3-01 — Broad GEX and synchronized ex-zone context](tasks/layer_3/L3-01-broad-and-ex-zone-gex.md) — Complete**
  - [ ] Implementation or deliverable completed
  - [ ] Tests / verification checks passed
  - [ ] Acceptance criteria verified
  - [ ] Reviewed and evidence recorded
  - Dependencies: [L1-01](tasks/layer_1/L1-01-oi-universe-and-gex-validation.md), [P04](tasks/storage/P04-domain-and-evaluation-schema.md).

- [ ] **[L3-02 — VIX/VIX1D changes, slopes and descriptive labels](tasks/layer_3/L3-02-volatility-context-and-labels.md) — Complete**
  - [ ] Implementation or deliverable completed
  - [ ] Tests / verification checks passed
  - [ ] Acceptance criteria verified
  - [ ] Reviewed and evidence recorded
  - Dependencies: [F03](tasks/foundation/F03-source-adapters-and-candle-normalization.md).

- [ ] **[L3-03 — SPY rolling and time-of-day RVOL](tasks/layer_3/L3-03-spy-relative-volume.md) — Complete**
  - [ ] Implementation or deliverable completed
  - [ ] Tests / verification checks passed
  - [ ] Acceptance criteria verified
  - [ ] Reviewed and evidence recorded
  - Dependencies: [F03](tasks/foundation/F03-source-adapters-and-candle-normalization.md).

- [ ] **[L3-04 — Immutable environmental context and late enrichment](tasks/layer_3/L3-04-immutable-environment-and-late-audit.md) — Complete**
  - [ ] Implementation or deliverable completed
  - [ ] Tests / verification checks passed
  - [ ] Acceptance criteria verified
  - [ ] Reviewed and evidence recorded
  - Dependencies: [L3-01](tasks/layer_3/L3-01-broad-and-ex-zone-gex.md), [L3-02](tasks/layer_3/L3-02-volatility-context-and-labels.md), [L3-03](tasks/layer_3/L3-03-spy-relative-volume.md), [L2-05](tasks/layer_2/L2-05-candidate-eligibility-and-publication.md), [P05](tasks/storage/P05-atomic-transitions-and-checkpoints.md).


## Layer 4 — Multi-timeframe alignment

- [ ] **[L4-01 — Spatial/timeframe matching and five-state resolution](tasks/layer_4/L4-01-matching-and-state-priority.md) — Complete**
  - [ ] Implementation or deliverable completed
  - [ ] Tests / verification checks passed
  - [ ] Acceptance criteria verified
  - [ ] Reviewed and evidence recorded
  - Dependencies: [L2-05](tasks/layer_2/L2-05-candidate-eligibility-and-publication.md).

- [ ] **[L4-02 — Immutable decision-time MTF records](tasks/layer_4/L4-02-decision-time-mtf-records.md) — Complete**
  - [ ] Implementation or deliverable completed
  - [ ] Tests / verification checks passed
  - [ ] Acceptance criteria verified
  - [ ] Reviewed and evidence recorded
  - Dependencies: [L4-01](tasks/layer_4/L4-01-matching-and-state-priority.md), [P05](tasks/storage/P05-atomic-transitions-and-checkpoints.md).

- [ ] **[L4-03 — Subsequent MTF events and follow-up deadlines](tasks/layer_4/L4-03-subsequent-comparison-events.md) — Complete**
  - [ ] Implementation or deliverable completed
  - [ ] Tests / verification checks passed
  - [ ] Acceptance criteria verified
  - [ ] Reviewed and evidence recorded
  - Dependencies: [L4-02](tasks/layer_4/L4-02-decision-time-mtf-records.md), [P05](tasks/storage/P05-atomic-transitions-and-checkpoints.md).


## Runtime integration and monitoring

- [ ] **[I01 — Live engine orchestration and session scheduling](tasks/integration/I01-live-engine-orchestration.md) — Complete**
  - [ ] Implementation or deliverable completed
  - [ ] Tests / verification checks passed
  - [ ] Acceptance criteria verified
  - [ ] Reviewed and evidence recorded
  - Dependencies: [F03](tasks/foundation/F03-source-adapters-and-candle-normalization.md), [P03](tasks/storage/P03-single-writer-and-readonly-viewers.md), [P07](tasks/storage/P07-durable-outbox-and-display-deduplication.md), [L1-04](tasks/layer_1/L1-04-snapshot-eligibility-and-session-observation.md), [L2-05](tasks/layer_2/L2-05-candidate-eligibility-and-publication.md), [L3-04](tasks/layer_3/L3-04-immutable-environment-and-late-audit.md), [L4-03](tasks/layer_4/L4-03-subsequent-comparison-events.md).

- [ ] **[I02 — Read-only monitoring and operator status](tasks/integration/I02-readonly-monitoring-and-operator-status.md) — Complete**
  - [ ] Implementation or deliverable completed
  - [ ] Tests / verification checks passed
  - [ ] Acceptance criteria verified
  - [ ] Reviewed and evidence recorded
  - Dependencies: [I01](tasks/integration/I01-live-engine-orchestration.md).


## Replay, evaluation and acceptance

- [ ] **[V01 — Deterministic recorded-input replay](tasks/validation/V01-deterministic-recorded-input-replay.md) — Complete**
  - [ ] Implementation or deliverable completed
  - [ ] Tests / verification checks passed
  - [ ] Acceptance criteria verified
  - [ ] Reviewed and evidence recorded
  - Dependencies: [P06](tasks/storage/P06-restart-recovery-and-reconciliation.md).

- [ ] **[V02 — Detached outcomes and descriptive reference moves](tasks/validation/V02-detached-outcomes-and-reference-moves.md) — Complete**
  - [ ] Implementation or deliverable completed
  - [ ] Tests / verification checks passed
  - [ ] Acceptance criteria verified
  - [ ] Reviewed and evidence recorded
  - Dependencies: [L2-05](tasks/layer_2/L2-05-candidate-eligibility-and-publication.md), [L4-03](tasks/layer_4/L4-03-subsequent-comparison-events.md), [P04](tasks/storage/P04-domain-and-evaluation-schema.md).

- [ ] **[V03 — Cross-layer acceptance and failure validation](tasks/validation/V03-cross-layer-acceptance-and-fault-injection.md) — Complete**
  - [ ] Implementation or deliverable completed
  - [ ] Tests / verification checks passed
  - [ ] Acceptance criteria verified
  - [ ] Reviewed and evidence recorded
  - Dependencies: [I02](tasks/integration/I02-readonly-monitoring-and-operator-status.md), [P08](tasks/storage/P08-backup-migrations-and-maintenance.md), [V01](tasks/validation/V01-deterministic-recorded-input-replay.md), [V02](tasks/validation/V02-detached-outcomes-and-reference-moves.md).

- [ ] **[V04 — Live observation validation and operational runbook](tasks/validation/V04-live-observation-validation-and-runbook.md) — Complete**
  - [ ] Implementation or deliverable completed
  - [ ] Tests / verification checks passed
  - [ ] Acceptance criteria verified
  - [ ] Reviewed and evidence recorded
  - Dependencies: [V03](tasks/validation/V03-cross-layer-acceptance-and-fault-injection.md).

## Release/operation readiness

Completing a layer's implementation does not by itself establish operational readiness. V03 owns cross-layer acceptance and failure validation; V04 owns actual live observation evidence and the operator runbook. Both remain open until their stated checks are performed. This checklist does not authorize broker execution or claim a validated trading edge.

