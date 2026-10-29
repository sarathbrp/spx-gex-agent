# Requirements Traceability

[Master task checklist](../TASKS.md)

**Baseline:** frozen v1.0 specifications, 2026-10-29.  
**Coverage:** 67 numbered sections and 72 acceptance cases assigned.  
**Verification status:** all acceptance boxes are initially unchecked.

Section ownership indicates where requirements are implemented or verified. Multiple tasks may contribute to a section. Acceptance ownership below identifies who must supply evidence; V03 consolidates cross-layer validation rather than replacing the owners' tests. Update a case only after all assigned owners record evidence, and retain links to the actual test/report.

The prose acceptance text below is copied from the frozen documents for traceability. Source specifications remain authoritative. Evidence of source availability is a separate prerequisite (F02); these mappings do not assert provider access or passing tests.

## Section coverage

### Layer 1

Source: [Layer 1 specification](../layer_1_zone_discovery.md).

| Section | Requirement area | Owning tasks |
|---|---|---|
| 1 | Purpose and boundaries | [F01](foundation/F01-project-contracts-and-test-harness.md) |
| 2 | Universe and measurement contract | [F01](foundation/F01-project-contracts-and-test-harness.md), [F02](foundation/F02-provider-and-reuse-verification.md), [L1-01](layer_1/L1-01-oi-universe-and-gex-validation.md) |
| 3 | Corridor, baseline and completeness | [F02](foundation/F02-provider-and-reuse-verification.md), [L1-01](layer_1/L1-01-oi-universe-and-gex-validation.md) |
| 4 | Deterministic greedy clustering | [L1-02](layer_1/L1-02-clustering-dominance-and-composition.md) |
| 5 | Dominance and candidate eligibility | [L1-02](layer_1/L1-02-clustering-dominance-and-composition.md) |
| 6 | Session phases | [F01](foundation/F01-project-contracts-and-test-harness.md), [F03](foundation/F03-source-adapters-and-candle-normalization.md), [L1-04](layer_1/L1-04-snapshot-eligibility-and-session-observation.md), [I01](integration/I01-live-engine-orchestration.md), [V04](validation/V04-live-observation-validation-and-runbook.md) |
| 7 | Causal timing and freshness | [F01](foundation/F01-project-contracts-and-test-harness.md), [F03](foundation/F03-source-adapters-and-candle-normalization.md), [P02](storage/P02-journal-ingestion-and-asof-reads.md), [L1-04](layer_1/L1-04-snapshot-eligibility-and-session-observation.md), [I01](integration/I01-live-engine-orchestration.md), [V01](validation/V01-deterministic-recorded-input-replay.md), [V04](validation/V04-live-observation-validation-and-runbook.md) |
| 8 | Frozen setups and refresh failures | [P06](storage/P06-restart-recovery-and-reconciliation.md), [L1-03](layer_1/L1-03-zone-versions-and-level-history.md), [L1-04](layer_1/L1-04-snapshot-eligibility-and-session-observation.md), [L2-05](layer_2/L2-05-candidate-eligibility-and-publication.md), [I01](integration/I01-live-engine-orchestration.md) |
| 9 | Identity, lineage and event history | [P04](storage/P04-domain-and-evaluation-schema.md), [P06](storage/P06-restart-recovery-and-reconciliation.md), [L1-03](layer_1/L1-03-zone-versions-and-level-history.md), [L2-02](layer_2/L2-02-episodes-contact-and-rearming.md), [I02](integration/I02-readonly-monitoring-and-operator-status.md) |
| 10 | Spatial ownership and bar evidence | [L1-02](layer_1/L1-02-clustering-dominance-and-composition.md), [L2-02](layer_2/L2-02-episodes-contact-and-rearming.md) |
| 11 | Persistence and restart contract | [P02](storage/P02-journal-ingestion-and-asof-reads.md), [P04](storage/P04-domain-and-evaluation-schema.md), [P05](storage/P05-atomic-transitions-and-checkpoints.md), [P06](storage/P06-restart-recovery-and-reconciliation.md), [L1-03](layer_1/L1-03-zone-versions-and-level-history.md), [I02](integration/I02-readonly-monitoring-and-operator-status.md), [V01](validation/V01-deterministic-recorded-input-replay.md), [V04](validation/V04-live-observation-validation-and-runbook.md) |
| 12 | Layer 2 handoff and acceptance examples | [L1-04](layer_1/L1-04-snapshot-eligibility-and-session-observation.md), [V03](validation/V03-cross-layer-acceptance-and-fault-injection.md) |

### Layer 2

Source: [Layer 2 specification](../layer_2_price_reactions.md).

| Section | Requirement area | Owning tasks |
|---|---|---|
| 1 | Responsibility and outputs | [L2-05](layer_2/L2-05-candidate-eligibility-and-publication.md) |
| 2 | Input and time contract | [F01](foundation/F01-project-contracts-and-test-harness.md), [F03](foundation/F03-source-adapters-and-candle-normalization.md), [L1-04](layer_1/L1-04-snapshot-eligibility-and-session-observation.md), [V01](validation/V01-deterministic-recorded-input-replay.md) |
| 3 | Research parameters | [F01](foundation/F01-project-contracts-and-test-harness.md), [L2-01](layer_2/L2-01-atr-warmup-and-frozen-buffers.md) |
| 4 | ATR definition and freezing | [F02](foundation/F02-provider-and-reuse-verification.md), [F03](foundation/F03-source-adapters-and-candle-normalization.md), [L2-01](layer_2/L2-01-atr-warmup-and-frozen-buffers.md) |
| 5 | Episode identity and indexing | [P05](storage/P05-atomic-transitions-and-checkpoints.md), [P06](storage/P06-restart-recovery-and-reconciliation.md), [L2-02](layer_2/L2-02-episodes-contact-and-rearming.md) |
| 6 | Approach, contact and spatial ownership | [L2-02](layer_2/L2-02-episodes-contact-and-rearming.md) |
| 7 | Testing, resolution and stall | [L2-02](layer_2/L2-02-episodes-contact-and-rearming.md), [L2-03](layer_2/L2-03-rejection-and-traversal-patterns.md), [L2-04](layer_2/L2-04-breakout-and-retest-watches.md) |
| 8 | Rejection geometry and immediate confirmation | [L2-03](layer_2/L2-03-rejection-and-traversal-patterns.md) |
| 9 | Re-arming | [P06](storage/P06-restart-recovery-and-reconciliation.md), [L2-02](layer_2/L2-02-episodes-contact-and-rearming.md) |
| 10 | Breakout initiation | [L2-04](layer_2/L2-04-breakout-and-retest-watches.md) |
| 11 | Retest watch and symmetric hold rules | [P06](storage/P06-restart-recovery-and-reconciliation.md), [L2-04](layer_2/L2-04-breakout-and-retest-watches.md) |
| 12 | Pattern, eligibility and session boundaries | [P06](storage/P06-restart-recovery-and-reconciliation.md), [L1-04](layer_1/L1-04-snapshot-eligibility-and-session-observation.md), [L2-05](layer_2/L2-05-candidate-eligibility-and-publication.md), [I01](integration/I01-live-engine-orchestration.md), [V01](validation/V01-deterministic-recorded-input-replay.md), [V04](validation/V04-live-observation-validation-and-runbook.md) |
| 13 | Immutable evidence and persistence | [P04](storage/P04-domain-and-evaluation-schema.md), [P05](storage/P05-atomic-transitions-and-checkpoints.md), [L2-03](layer_2/L2-03-rejection-and-traversal-patterns.md), [L2-05](layer_2/L2-05-candidate-eligibility-and-publication.md), [V01](validation/V01-deterministic-recorded-input-replay.md), [V04](validation/V04-live-observation-validation-and-runbook.md) |
| 14 | Detached outcome evaluation and causal comparison | [V02](validation/V02-detached-outcomes-and-reference-moves.md) |
| 15 | Acceptance cases and remaining scope | [V03](validation/V03-cross-layer-acceptance-and-fault-injection.md) |

### Layer 3

Source: [Layer 3 specification](../layer_3_environmental_context.md).

| Section | Requirement area | Owning tasks |
|---|---|---|
| 1 | Baseline policy and responsibility | [L3-04](layer_3/L3-04-immutable-environment-and-late-audit.md), [I01](integration/I01-live-engine-orchestration.md) |
| 2 | Candidate clocks and immutable observations | [L3-04](layer_3/L3-04-immutable-environment-and-late-audit.md), [I01](integration/I01-live-engine-orchestration.md), [V01](validation/V01-deterministic-recorded-input-replay.md) |
| 3 | GEX universe and synchronization | [F02](foundation/F02-provider-and-reuse-verification.md), [L1-01](layer_1/L1-01-oi-universe-and-gex-validation.md), [L3-01](layer_3/L3-01-broad-and-ex-zone-gex.md) |
| 4 | Independent baseline and broad quality | [F02](foundation/F02-provider-and-reuse-verification.md), [L1-01](layer_1/L1-01-oi-universe-and-gex-validation.md), [L3-01](layer_3/L3-01-broad-and-ex-zone-gex.md) |
| 5 | Broad and ex-zone mathematics | [L3-01](layer_3/L3-01-broad-and-ex-zone-gex.md) |
| 6 | Volatility source and alignment | [F02](foundation/F02-provider-and-reuse-verification.md), [F03](foundation/F03-source-adapters-and-candle-normalization.md), [L3-02](layer_3/L3-02-volatility-context-and-labels.md) |
| 7 | Delta and normalized slope | [F03](foundation/F03-source-adapters-and-candle-normalization.md), [L3-02](layer_3/L3-02-volatility-context-and-labels.md) |
| 8 | Directional consistency labels | [L3-02](layer_3/L3-02-volatility-context-and-labels.md) |
| 9 | SPY volume input contract | [F02](foundation/F02-provider-and-reuse-verification.md), [F03](foundation/F03-source-adapters-and-candle-normalization.md), [L3-03](layer_3/L3-03-spy-relative-volume.md) |
| 10 | Rolling RVOL | [F03](foundation/F03-source-adapters-and-candle-normalization.md), [L3-03](layer_3/L3-03-spy-relative-volume.md) |
| 11 | Time-of-day RVOL | [F03](foundation/F03-source-adapters-and-candle-normalization.md), [L3-03](layer_3/L3-03-spy-relative-volume.md) |
| 12 | Environmental record and status contract | [P04](storage/P04-domain-and-evaluation-schema.md), [L3-04](layer_3/L3-04-immutable-environment-and-late-audit.md), [I02](integration/I02-readonly-monitoring-and-operator-status.md) |
| 13 | Persistence, replay and evaluation boundaries | [P06](storage/P06-restart-recovery-and-reconciliation.md), [L3-04](layer_3/L3-04-immutable-environment-and-late-audit.md), [V01](validation/V01-deterministic-recorded-input-replay.md), [V02](validation/V02-detached-outcomes-and-reference-moves.md), [V04](validation/V04-live-observation-validation-and-runbook.md) |
| 14 | Acceptance examples | [V03](validation/V03-cross-layer-acceptance-and-fault-injection.md), [V04](validation/V04-live-observation-validation-and-runbook.md) |

### Layer 4

Source: [Layer 4 specification](../layer_4_multitimeframe_alignment.md).

| Section | Requirement area | Owning tasks |
|---|---|---|
| 1 | Baseline policy | [L4-01](layer_4/L4-01-matching-and-state-priority.md), [I01](integration/I01-live-engine-orchestration.md) |
| 2 | Identity and matching scope | [L4-01](layer_4/L4-01-matching-and-state-priority.md) |
| 3 | Two clocks and the 300-second window | [L4-01](layer_4/L4-01-matching-and-state-priority.md), [I01](integration/I01-live-engine-orchestration.md), [V01](validation/V01-deterministic-recorded-input-replay.md) |
| 4 | The relevant counterpart interval | [L4-01](layer_4/L4-01-matching-and-state-priority.md) |
| 5 | Health evidence and state priority | [L4-01](layer_4/L4-01-matching-and-state-priority.md) |
| 6 | Multiple counterpart candidates and family alignment | [L4-01](layer_4/L4-01-matching-and-state-priority.md) |
| 7 | Forming-candle isolation | [L4-01](layer_4/L4-01-matching-and-state-priority.md), [I02](integration/I02-readonly-monitoring-and-operator-status.md) |
| 8 | Immutable decision-time record | [P04](storage/P04-domain-and-evaluation-schema.md), [L4-02](layer_4/L4-02-decision-time-mtf-records.md), [I02](integration/I02-readonly-monitoring-and-operator-status.md), [V01](validation/V01-deterministic-recorded-input-replay.md) |
| 9 | Subsequent comparison events | [P04](storage/P04-domain-and-evaluation-schema.md), [P06](storage/P06-restart-recovery-and-reconciliation.md), [L4-03](layer_4/L4-03-subsequent-comparison-events.md), [I02](integration/I02-readonly-monitoring-and-operator-status.md), [V04](validation/V04-live-observation-validation-and-runbook.md) |
| 10 | Reference movement and waiting-policy evaluation | [V02](validation/V02-detached-outcomes-and-reference-moves.md) |
| 11 | Worked timing cases | [L4-03](layer_4/L4-03-subsequent-comparison-events.md) |
| 12 | Persistence, reconstruction and acceptance | [P04](storage/P04-domain-and-evaluation-schema.md), [P06](storage/P06-restart-recovery-and-reconciliation.md), [L4-02](layer_4/L4-02-decision-time-mtf-records.md), [L4-03](layer_4/L4-03-subsequent-comparison-events.md), [V01](validation/V01-deterministic-recorded-input-replay.md), [V03](validation/V03-cross-layer-acceptance-and-fault-injection.md), [V04](validation/V04-live-observation-validation-and-runbook.md) |

### Persistence

Source: [Persistence specification](../persistent_state_and_storage.md).

| Section | Requirement area | Owning tasks |
|---|---|---|
| 1 | Goal and scope | [F01](foundation/F01-project-contracts-and-test-harness.md) |
| 2 | Agreed deployment | [P01](storage/P01-sqlite-bootstrap-and-source-schema.md), [P03](storage/P03-single-writer-and-readonly-viewers.md), [P08](storage/P08-backup-migrations-and-maintenance.md), [I01](integration/I01-live-engine-orchestration.md), [I02](integration/I02-readonly-monitoring-and-operator-status.md) |
| 3 | One storage module with a small interface | [F01](foundation/F01-project-contracts-and-test-harness.md), [P02](storage/P02-journal-ingestion-and-asof-reads.md), [P05](storage/P05-atomic-transitions-and-checkpoints.md), [I02](integration/I02-readonly-monitoring-and-operator-status.md), [V02](validation/V02-detached-outcomes-and-reference-moves.md) |
| 4 | Identities and clocks | [F01](foundation/F01-project-contracts-and-test-harness.md), [F03](foundation/F03-source-adapters-and-candle-normalization.md), [P01](storage/P01-sqlite-bootstrap-and-source-schema.md), [P02](storage/P02-journal-ingestion-and-asof-reads.md), [V01](validation/V01-deterministic-recorded-input-replay.md) |
| 5 | Logical schema: metadata and source evidence | [F02](foundation/F02-provider-and-reuse-verification.md), [F03](foundation/F03-source-adapters-and-candle-normalization.md), [P01](storage/P01-sqlite-bootstrap-and-source-schema.md), [P02](storage/P02-journal-ingestion-and-asof-reads.md), [P08](storage/P08-backup-migrations-and-maintenance.md), [L1-01](layer_1/L1-01-oi-universe-and-gex-validation.md) |
| 6 | Logical schema: zones and lineage | [P04](storage/P04-domain-and-evaluation-schema.md), [L1-03](layer_1/L1-03-zone-versions-and-level-history.md) |
| 7 | Logical schema: Layer 2 state and decisions | [P04](storage/P04-domain-and-evaluation-schema.md), [P05](storage/P05-atomic-transitions-and-checkpoints.md), [L1-03](layer_1/L1-03-zone-versions-and-level-history.md), [L2-05](layer_2/L2-05-candidate-eligibility-and-publication.md) |
| 8 | Logical schema: Layers 3–4 and detached evaluation | [P04](storage/P04-domain-and-evaluation-schema.md), [P07](storage/P07-durable-outbox-and-display-deduplication.md), [L3-04](layer_3/L3-04-immutable-environment-and-late-audit.md), [L4-02](layer_4/L4-02-decision-time-mtf-records.md), [V02](validation/V02-detached-outcomes-and-reference-moves.md) |
| 9 | State ownership and transitions | [P05](storage/P05-atomic-transitions-and-checkpoints.md), [P06](storage/P06-restart-recovery-and-reconciliation.md), [I01](integration/I01-live-engine-orchestration.md) |
| 10 | Atomic transition protocol | [P02](storage/P02-journal-ingestion-and-asof-reads.md), [P05](storage/P05-atomic-transitions-and-checkpoints.md), [P07](storage/P07-durable-outbox-and-display-deduplication.md), [L2-05](layer_2/L2-05-candidate-eligibility-and-publication.md), [I01](integration/I01-live-engine-orchestration.md) |
| 11 | Duplicate prevention and publication semantics | [P02](storage/P02-journal-ingestion-and-asof-reads.md), [P03](storage/P03-single-writer-and-readonly-viewers.md), [P05](storage/P05-atomic-transitions-and-checkpoints.md), [P07](storage/P07-durable-outbox-and-display-deduplication.md), [L2-05](layer_2/L2-05-candidate-eligibility-and-publication.md), [I02](integration/I02-readonly-monitoring-and-operator-status.md), [V04](validation/V04-live-observation-validation-and-runbook.md) |
| 12 | Recovery and missing intervals | [P03](storage/P03-single-writer-and-readonly-viewers.md), [P06](storage/P06-restart-recovery-and-reconciliation.md), [P07](storage/P07-durable-outbox-and-display-deduplication.md), [I01](integration/I01-live-engine-orchestration.md), [V01](validation/V01-deterministic-recorded-input-replay.md), [V04](validation/V04-live-observation-validation-and-runbook.md) |
| 13 | Replay isolation, indexes and operation | [P01](storage/P01-sqlite-bootstrap-and-source-schema.md), [P03](storage/P03-single-writer-and-readonly-viewers.md), [P06](storage/P06-restart-recovery-and-reconciliation.md), [P08](storage/P08-backup-migrations-and-maintenance.md), [I02](integration/I02-readonly-monitoring-and-operator-status.md), [V01](validation/V01-deterministic-recorded-input-replay.md), [V04](validation/V04-live-observation-validation-and-runbook.md) |
| 14 | Acceptance and fault-injection cases | [P08](storage/P08-backup-migrations-and-maintenance.md), [V03](validation/V03-cross-layer-acceptance-and-fault-injection.md), [V04](validation/V04-live-observation-validation-and-runbook.md) |

## Frozen acceptance checklist

### Layer 1 acceptance cases

Source: [Layer 1](../layer_1_zone_discovery.md).

- [ ] **L1-AC01** — +3B call and -3B put sensitivity produce 6B gross, zero net and balanced composition.
  - Owners: [L1-01](layer_1/L1-01-oi-universe-and-gex-validation.md). Evidence: **Not run**.

- [ ] **L1-AC02** — Equal-gross seeds and competing neighbors yield repeatable, non-overlapping strike assignments.
  - Owners: [L1-02](layer_1/L1-02-clustering-dominance-and-composition.md). Evidence: **Not run**.

- [ ] **L1-AC03** — A 4B seed absorbs a 2B neighbor at the inclusive 50% boundary; no third strike is absorbed.
  - Owners: [L1-02](layer_1/L1-02-clustering-dominance-and-composition.md). Evidence: **Not run**.

- [ ] **L1-AC04** — Missing quote contracts remain in the independently established OI denominator; exactly 90% passes coverage.
  - Owners: [L1-01](layer_1/L1-01-oi-universe-and-gex-validation.md). Evidence: **Not run**.

- [ ] **L1-AC05** — Zero bid is never automatically converted to zero gamma; missing baseline is never zero exposure.
  - Owners: [L1-01](layer_1/L1-01-oi-universe-and-gex-validation.md). Evidence: **Not run**.

- [ ] **L1-AC06** — A 10:05:18 snapshot cannot influence a 10:05 decision or claim the earlier portion of a forming candle.
  - Owners: [L1-04](layer_1/L1-04-snapshot-eligibility-and-session-observation.md). Evidence: **Not run**.

- [ ] **L1-AC07** — TTL allows exactly 360 seconds and blocks greater ages; an incomplete refresh cannot silently fall back to healthy status.
  - Owners: [L1-04](layer_1/L1-04-snapshot-eligibility-and-session-observation.md). Evidence: **Not run**.

- [ ] **L1-AC08** — A blocked trigger is not released automatically after recovery.
  - Owners: [L2-05](layer_2/L2-05-candidate-eligibility-and-publication.md). Evidence: **Not run**.

- [ ] **L1-AC09** — Refreshes, seed changes, merges and splits preserve original events and frozen setup boundaries.
  - Owners: [L1-03](layer_1/L1-03-zone-versions-and-level-history.md). Evidence: **Not run**.

- [ ] **L1-AC10** — Restart and repeated ingestion produce the same event identities and no duplicate counts.
  - Owners: [P06](storage/P06-restart-recovery-and-reconciliation.md). Evidence: **Not run**.

- [ ] **L1-AC11** — Exact boundary ownership is deterministic, and multi-zone spans do not invent an intrabar path.
  - Owners: [L2-02](layer_2/L2-02-episodes-contact-and-rearming.md). Evidence: **Not run**.

- [ ] **L1-AC12** — Sub-threshold clusters are retained; independent 1m/5m evaluation cannot access future outcomes.
  - Owners: [L1-02](layer_1/L1-02-clustering-dominance-and-composition.md), [V02](validation/V02-detached-outcomes-and-reference-moves.md). Evidence: **Not run**.


### Layer 2 acceptance cases

Source: [Layer 2](../layer_2_price_reactions.md).

- [ ] **L2-AC01** — One multi-candle encounter creates one episode/test index; re-arming increments neither.
  - Owners: [L2-02](layer_2/L2-02-episodes-contact-and-rearming.md). Evidence: **Not run**.

- [ ] **L2-AC02** — Index assignment survives late-session termination and idempotent replay; related-zone counts do not silently merge.
  - Owners: [L2-02](layer_2/L2-02-episodes-contact-and-rearming.md). Evidence: **Not run**.

- [ ] **L2-AC03** — Upper/lower boundary equality is handled symmetrically; a whole-zone gap is not a traded touch.
  - Owners: [L2-02](layer_2/L2-02-episodes-contact-and-rearming.md). Evidence: **Not run**.

- [ ] **L2-AC04** — Range exactly 0.50, wick exactly 0.40 and close placement exactly 2/3 pass; values below fail.
  - Owners: [L2-03](layer_2/L2-03-rejection-and-traversal-patterns.md). Evidence: **Not run**.

- [ ] **L2-AC05** — The 30.9% upper-wick example produces no rejection candidate.
  - Owners: [L2-03](layer_2/L2-03-rejection-and-traversal-patterns.md). Evidence: **Not run**.

- [ ] **L2-AC06** — Symmetric full-zone recoveries are excluded from ordinary rejections.
  - Owners: [L2-03](layer_2/L2-03-rejection-and-traversal-patterns.md). Evidence: **Not run**.

- [ ] **L2-AC07** — ATR uses the recorded 130-bar seed/window and prior close; missing inputs do not activate point-floor fallback.
  - Owners: [L2-01](layer_2/L2-01-atr-warmup-and-frozen-buffers.md). Evidence: **Not run**.

- [ ] **L2-AC08** — A resolution candle does not count toward re-arming; side changes cannot complete a mixed-side streak.
  - Owners: [L2-02](layer_2/L2-02-episodes-contact-and-rearming.md). Evidence: **Not run**.

- [ ] **L2-AC09** — Stalls resolve on the 11th/6th consecutive inside close, not on the 10th/5th.
  - Owners: [L2-02](layer_2/L2-02-episodes-contact-and-rearming.md). Evidence: **Not run**.

- [ ] **L2-AC10** — A first-next-candle retest can qualify; the breakout candle cannot retest itself.
  - Owners: [L2-04](layer_2/L2-04-breakout-and-retest-watches.md). Evidence: **Not run**.

- [ ] **L2-AC11** — Unbuffered closes and already-cleared prior closes cannot create later drift-based breakout watches.
  - Owners: [L2-04](layer_2/L2-04-breakout-and-retest-watches.md). Evidence: **Not run**.

- [ ] **L2-AC12** — Retests on bars 15/6 can qualify; bars 16/7 cannot. Missing intervals invalidate rather than extend the watch.
  - Owners: [L2-04](layer_2/L2-04-breakout-and-retest-watches.md). Evidence: **Not run**.

- [ ] **L2-AC13** — Exactly 50% penetration is allowed; deeper wicks invalidate despite recovery. Failed-close rules mirror correctly.
  - Owners: [L2-04](layer_2/L2-04-breakout-and-retest-watches.md). Evidence: **Not run**.

- [ ] **L2-AC14** — Each watch has at most one candidate; blocked triggers are not queued; consumed records survive restart.
  - Owners: [L2-04](layer_2/L2-04-breakout-and-retest-watches.md), [L2-05](layer_2/L2-05-candidate-eligibility-and-publication.md), [P06](storage/P06-restart-recovery-and-reconciliation.md). Evidence: **Not run**.

- [ ] **L2-AC15** — Morning observations persist, but candidates require a fresh confirmation candle starting at or after 10:00.
  - Owners: [L2-05](layer_2/L2-05-candidate-eligibility-and-publication.md). Evidence: **Not run**.

- [ ] **L2-AC16** — Current health uses a current snapshot, while geometry/ATR remain frozen; unavailable historical inputs cannot be borrowed from the future.
  - Owners: [L1-04](layer_1/L1-04-snapshot-eligibility-and-session-observation.md), [L2-01](layer_2/L2-01-atr-warmup-and-frozen-buffers.md), [L2-05](layer_2/L2-05-candidate-eligibility-and-publication.md). Evidence: **Not run**.

- [ ] **L2-AC17** — The 1m and 5m paths remain independent; shared-boundary observations do not generate duplicate ownership events.
  - Owners: [L2-02](layer_2/L2-02-episodes-contact-and-rearming.md), [I01](integration/I01-live-engine-orchestration.md). Evidence: **Not run**.

- [ ] **L2-AC18** — Next-bar confirmations remain detached; delayed comparisons cannot enter at the earlier t+1 open or infer option fills.
  - Owners: [V02](validation/V02-detached-outcomes-and-reference-moves.md). Evidence: **Not run**.


### Layer 3 acceptance cases

Source: [Layer 3](../layer_3_environmental_context.md).

- [ ] **L3-AC01** — An eligible 10:20 5m candidate survives unavailable rolling RVOL, absent VIX1D and incomplete broad GEX; each factor carries its own reason.
  - Owners: [L3-04](layer_3/L3-04-immutable-environment-and-late-audit.md). Evidence: **Not run**.

- [ ] **L3-AC02** — Broad coverage of exactly 85% passes; below 85% nulls broad environmental metrics without changing local 90% requirements or candidate eligibility.
  - Owners: [L3-01](layer_3/L3-01-broad-and-ex-zone-gex.md). Evidence: **Not run**.

- [ ] **L3-AC03** — Missing independent OI baseline is unavailable, never a zero-exposure observation. Zero bid does not imply zero modeled gamma.
  - Owners: [L1-01](layer_1/L1-01-oi-universe-and-gex-validation.md). Evidence: **Not run**.

- [ ] **L3-AC04** — Synchronized zone subtraction uses the broad snapshot, not frozen episode-time dollar values. Out-of-corridor members are explicit.
  - Owners: [L3-01](layer_3/L3-01-broad-and-ex-zone-gex.md). Evidence: **Not run**.

- [ ] **L3-AC05** — G=100, N=-20, G_C=20, N_C=10 produces balance -0.20, share 0.20, synchronized bias +0.50 and ex-zone balance -0.375.
  - Owners: [L3-01](layer_3/L3-01-broad-and-ex-zone-gex.md). Evidence: **Not run**.

- [ ] **L3-AC06** — Zero gross denominators yield null ratios; zero net with positive gross yields valid zero. Unavailable inputs never become a congruent label.
  - Owners: [L3-01](layer_3/L3-01-broad-and-ex-zone-gex.md). Evidence: **Not run**.

- [ ] **L3-AC07** — A late snapshot or late VIX/SPY bar only appears in enrichment audit. Later computation cannot move the original input cutoff.
  - Owners: [L3-04](layer_3/L3-04-immutable-environment-and-late-audit.md). Evidence: **Not run**.

- [ ] **L3-AC08** — Exact 15-minute endpoints and five consecutive slope observations are required; 1m/5m slopes use 4/20-minute elapsed spans and point-per-minute units.
  - Owners: [L3-02](layer_3/L3-02-volatility-context-and-labels.md). Evidence: **Not run**.

- [ ] **L3-AC09** — CALL delta -0.15 with negative slope is supportive; PUT delta +0.25 with positive slope is supportive. Zero or contrary slope is inconclusive; missing required input is unavailable.
  - Owners: [L3-02](layer_3/L3-02-volatility-context-and-labels.md). Evidence: **Not run**.

- [ ] **L3-AC10** — Missing VIX does not erase valid VIX1D classification. Classification never vetoes a candidate.
  - Owners: [L3-02](layer_3/L3-02-volatility-context-and-labels.md). Evidence: **Not run**.

- [ ] **L3-AC11** — Rolling RVOL first completes at 09:51 on 1m and 11:15 on 5m; current volume is excluded from its 20-bar denominator.
  - Owners: [L3-03](layer_3/L3-03-spy-relative-volume.md). Evidence: **Not run**.

- [ ] **L3-AC12** — A zero numerator can yield valid zero RVOL; a zero denominator cannot. Missing bars are not zero-volume bars.
  - Owners: [L3-03](layer_3/L3-03-spy-relative-volume.md). Evidence: **Not run**.

- [ ] **L3-AC13** — TOD uses the fixed prior-ten-session list: eight valid matches pass, seven fail, and no eleventh-session backfill occurs.
  - Owners: [L3-03](layer_3/L3-03-spy-relative-volume.md). Evidence: **Not run**.

- [ ] **L3-AC14** — An early-close session can supply a matching morning interval but not a post-close interval. Current-day volume never enters historical TOD references.
  - Owners: [L3-03](layer_3/L3-03-spy-relative-volume.md). Evidence: **Not run**.

- [ ] **L3-AC15** — Repeated processing and restart preserve original context, missingness and candidate identity; later outcomes cannot alter evidence.
  - Owners: [L3-04](layer_3/L3-04-immutable-environment-and-late-audit.md), [P06](storage/P06-restart-recovery-and-reconciliation.md). Evidence: **Not run**.


### Layer 4 acceptance cases

Source: [Layer 4](../layer_4_multitimeframe_alignment.md).

- [ ] **L4-AC01** — Every eligible Layer 2 candidate survives all five comparison states and all Layer 4 errors.
  - Owners: [L4-01](layer_4/L4-01-matching-and-state-priority.md), [L4-02](layer_4/L4-02-decision-time-mtf-records.md), [I01](integration/I01-live-engine-orchestration.md). Evidence: **Not run**.

- [ ] **L4-AC02** — Same underlying/session/universe/configuration and exact member sets are required; partial overlap cannot establish agreement.
  - Owners: [L4-01](layer_4/L4-01-matching-and-state-priority.md). Evidence: **Not run**.

- [ ] **L4-AC03** — Each bound at 0.01-point difference passes tolerance; larger differences fail without transitive geometry merging.
  - Owners: [L4-01](layer_4/L4-01-matching-and-state-priority.md). Evidence: **Not run**.

- [ ] **L4-AC04** — Both availability and completion windows are enforced; exact 300-second boundaries pass and greater ages fail.
  - Owners: [L4-01](layer_4/L4-01-matching-and-state-priority.md). Evidence: **Not run**.

- [ ] **L4-AC05** — Identical wall-clock timestamps obey recorded event ordering; no counterpart processed later leaks into an earlier decision.
  - Owners: [L4-02](layer_4/L4-02-decision-time-mtf-records.md). Evidence: **Not run**.

- [ ] **L4-AC06** — Missing required data wins summary priority but does not delete known evidence.
  - Owners: [L4-01](layer_4/L4-01-matching-and-state-priority.md). Evidence: **Not run**.

- [ ] **L4-AC07** — When same- and opposite-side matches coexist, conflict wins the summary, all matches remain stored, and the representative tie-break is deterministic.
  - Owners: [L4-01](layer_4/L4-01-matching-and-state-priority.md). Evidence: **Not run**.

- [ ] **L4-AC08** — Homogeneous and heterogeneous agreement are distinguished; absence of a counterpart never sets spatial match to true.
  - Owners: [L4-01](layer_4/L4-01-matching-and-state-priority.md). Evidence: **Not run**.

- [ ] **L4-AC09** — The relevant interval ending at a shared boundary is selected instead of the newly forming interval; complete no-candidate evaluation produces NO_MATCH.
  - Owners: [L4-01](layer_4/L4-01-matching-and-state-priority.md). Evidence: **Not run**.

- [ ] **L4-AC10** — Completed recent agreement outranks pending status; forming wick shape never creates agreement/conflict or a veto.
  - Owners: [L4-01](layer_4/L4-01-matching-and-state-priority.md). Evidence: **Not run**.

- [ ] **L4-AC11** — Later events follow candidates from every original state, with the original rows immutable.
  - Owners: [L4-03](layer_4/L4-03-subsequent-comparison-events.md). Evidence: **Not run**.

- [ ] **L4-AC12** — Subsequent same-close delivery, genuinely later confirmation, beyond-window arrivals and no-match completion remain distinguishable.
  - Owners: [L4-03](layer_4/L4-03-subsequent-comparison-events.md). Evidence: **Not run**.

- [ ] **L4-AC13** — CALL upward and PUT downward reference moves have the same positive direction-adjusted sign; neither is labeled an execution fill or slippage.
  - Owners: [V02](validation/V02-detached-outcomes-and-reference-moves.md). Evidence: **Not run**.

- [ ] **L4-AC14** — Waiting policies cannot use pre-confirmation opens, later-selected best events or future outcomes in decision matching.
  - Owners: [V02](validation/V02-detached-outcomes-and-reference-moves.md). Evidence: **Not run**.

- [ ] **L4-AC15** — Restart/replay preserve candidate IDs, comparison cutoffs, audit deduplication and deadlines without re-emission.
  - Owners: [P06](storage/P06-restart-recovery-and-reconciliation.md). Evidence: **Not run**.


### Persistence acceptance cases

Source: [Persistence](../persistent_state_and_storage.md).

- [ ] **P-AC01** — Starting two engines against the same store yields one writer; extra viewer sessions produce no additional source polling or candidate rows.
  - Owners: [P03](storage/P03-single-writer-and-readonly-viewers.md). Evidence: **Not run**.

- [ ] **P-AC02** — Repeating one captured input or transition creates no duplicate snapshot revision, episode, count, watch, candidate or audit event.
  - Owners: [P02](storage/P02-journal-ingestion-and-asof-reads.md), [P05](storage/P05-atomic-transitions-and-checkpoints.md). Evidence: **Not run**.

- [ ] **P-AC03** — Two candidates on the same timestamp but different valid scopes remain distinct.
  - Owners: [P04](storage/P04-domain-and-evaluation-schema.md). Evidence: **Not run**.

- [ ] **P-AC04** — A crash before transition commit leaves no partial candidate, advanced checkpoint or consumed watch. A crash after commit restores all of them together.
  - Owners: [P05](storage/P05-atomic-transitions-and-checkpoints.md). Evidence: **Not run**.

- [ ] **P-AC05** — A crash after display but before acknowledgment may redeliver the same stable event ID, never create a new candidate or fabricate exactly-once delivery.
  - Owners: [P07](storage/P07-durable-outbox-and-display-deduplication.md). Evidence: **Not run**.

- [ ] **P-AC06** — Frozen geometry, ATR, test scope and original deadlines survive restart; history counts agree with journal reconstruction.
  - Owners: [P06](storage/P06-restart-recovery-and-reconciliation.md). Evidence: **Not run**.

- [ ] **P-AC07** — A snapshot with missing contracts retains the independent OI denominator and failure evidence; local and broad health stay separate.
  - Owners: [L1-01](layer_1/L1-01-oi-universe-and-gex-validation.md), [L3-01](layer_3/L3-01-broad-and-ex-zone-gex.md). Evidence: **Not run**.

- [ ] **P-AC08** — Same-close 1m/5m races reproduce original comparison states from event ordering; later input cannot overwrite them.
  - Owners: [L4-02](layer_4/L4-02-decision-time-mtf-records.md), [V01](validation/V01-deterministic-recorded-input-replay.md). Evidence: **Not run**.

- [ ] **P-AC09** — Price-data gaps invalidate sequences; optional context outages do not remove eligible Layer 2 candidates.
  - Owners: [L2-05](layer_2/L2-05-candidate-eligibility-and-publication.md), [L3-04](layer_3/L3-04-immutable-environment-and-late-audit.md). Evidence: **Not run**.

- [ ] **P-AC10** — Late corrected bars and snapshots create revisions without changing original input visibility.
  - Owners: [P02](storage/P02-journal-ingestion-and-asof-reads.md). Evidence: **Not run**.

- [ ] **P-AC11** — A replay writes only to its own namespace and cannot generate live delivery intents.
  - Owners: [V01](validation/V01-deterministic-recorded-input-replay.md). Evidence: **Not run**.

- [ ] **P-AC12** — A restore can rebuild projections, follow foreign-key links and recompute stored decision evidence without reading future outcomes.
  - Owners: [P08](storage/P08-backup-migrations-and-maintenance.md). Evidence: **Not run**.

## Additional operational evidence

- [ ] F02 provider capability, independent OI and valuation prerequisites are verified with retained evidence.
- [ ] V01 recorded-input replay is distinguished from retrospective reconstruction and leaves live streams untouched.
- [ ] V03 consolidates all acceptance evidence and unresolved defects.
- [ ] V04 records actual feed-to-observation behavior, restart/viewer checks and unobserved scenarios without execution claims.

Do not mark these complete based on documentation creation, fixture-only tests or unavailable live-market conditions.

