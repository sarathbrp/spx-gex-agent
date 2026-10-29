# L4-02 — Immutable decision-time MTF records

**Status:** NOT_STARTED  
**Group:** layer_4  
**Task type:** Implementation  
**Owner:** Unassigned  
**Created:** 2026-10-29  
**Specification baseline:** Frozen v1.0

[Master checklist](../../TASKS.md) · [Requirements traceability](../TRACEABILITY.md)

## Objective

Persist exactly what each candidate could compare at its original cutoff.

## Completion checklist

- [ ] Implementation or deliverable completed
- [ ] Tests / verification checks passed
- [ ] Acceptance criteria verified
- [ ] Reviewed and evidence recorded
- [ ] Complete — master index updated

Every box starts unchecked. Creating this plan is not implementation or testing. Update this task and its master-index entry together; do not mark Complete until required checks and acceptance evidence are recorded. A verification task completes its stated evidence deliverable rather than claiming application code was implemented.

## Authoritative requirements

- [Layer 4 specification](../../layer_4_multitimeframe_alignment.md) — sections 8, 12.
- [Persistence specification](../../persistent_state_and_storage.md) — sections 8.

The linked frozen specifications take precedence over this task summary. Do not silently introduce new strategy rules. Record any material unresolved input or implementation decision below before dependent work proceeds.

## Dependencies

- [ ] [L4-01 — Spatial/timeframe matching and five-state resolution](../layer_4/L4-01-matching-and-state-priority.md) completed, or an explicitly documented interface prerequisite is available.
- [ ] [P05 — Atomic transitions, projections and checkpoints](../storage/P05-atomic-transitions-and-checkpoints.md) completed, or an explicitly documented interface prerequisite is available.

Dependencies describe prerequisites, not permission to change another task's scope. Tests may use agreed fixtures/interfaces before full integration; acceptance still requires the actual dependencies and relevant integration checks.

## Implementation / deliverable checklist

- [ ] Store one original record per candidate/comparison configuration with health, interval, all matches and representative evidence.
- [ ] Enforce recorded time/cursor even when optional comparison calculation completes later.
- [ ] Preserve original unavailable/pending state and diagnostic evidence under errors/retries.
- [ ] Expose typed comparison reads without forming telemetry or outcome fields.

## Required tests and verification

- [ ] Race 1m/5m publication at the same wall-clock close and reproduce both original viewpoints.
- [ ] Retry/restart comparison publication and verify one immutable result per key.
- [ ] Change later evidence and confirm original state/matches remain unchanged and candidate publication is unblocked.

Run focused tests for this task and relevant integration/regression checks. Record actual commands, versions and results; an unexecuted test plan is not a passing result. Follow applicable repository testing and change-process instructions during implementation.

## Assigned frozen acceptance cases

- [ ] **L4-AC01** — Every eligible Layer 2 candidate survives all five comparison states and all Layer 4 errors. ([source](../../layer_4_multitimeframe_alignment.md)).
- [ ] **L4-AC05** — Identical wall-clock timestamps obey recorded event ordering; no counterpart processed later leaks into an earlier decision. ([source](../../layer_4_multitimeframe_alignment.md)).
- [ ] **P-AC08** — Same-close 1m/5m races reproduce original comparison states from event ordering; later input cannot overwrite them. ([source](../../persistent_state_and_storage.md)).

Shared cases require evidence from all listed owners in the traceability checklist. Do not close a shared acceptance case based only on this task's portion.

## Deliverables

Original MTF context consumer and persistence tests.

Implement only within the new `spx-gex-agent` application unless a separate instruction authorizes otherwise. Record actual implementation/test paths when work begins; this plan does not prescribe unreviewed module names.

## Scope limits

No retroactive update or awaiting counterpart data before Layer 2 emits.

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

