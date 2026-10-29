# L2-04 — Fresh breakout and retest watch lifecycle

**Status:** NOT_STARTED  
**Group:** layer_2  
**Task type:** Implementation  
**Owner:** Unassigned  
**Created:** 2026-10-29  
**Specification baseline:** Frozen v1.0

[Master checklist](../../TASKS.md) · [Requirements traceability](../TRACEABILITY.md)

## Objective

Track one buffered break and later retest with deterministic invalidation and consumption.

## Completion checklist

- [ ] Implementation or deliverable completed
- [ ] Tests / verification checks passed
- [ ] Acceptance criteria verified
- [ ] Reviewed and evidence recorded
- [ ] Complete — master index updated

Every box starts unchecked. Creating this plan is not implementation or testing. Update this task and its master-index entry together; do not mark Complete until required checks and acceptance evidence are recorded. A verification task completes its stated evidence deliverable rather than claiming application code was implemented.

## Authoritative requirements

- [Layer 2 specification](../../layer_2_price_reactions.md) — sections 7, 10, 11.

The linked frozen specifications take precedence over this task summary. Do not silently introduce new strategy rules. Record any material unresolved input or implementation decision below before dependent work proceeds.

## Dependencies

- [ ] [L2-02 — Interaction episodes, contact ownership and re-arming](../layer_2/L2-02-episodes-contact-and-rearming.md) completed, or an explicitly documented interface prerequisite is available.

Dependencies describe prerequisites, not permission to change another task's scope. Tests may use agreed fixtures/interfaces before full integration; acceptance still requires the actual dependencies and relevant integration checks.

## Implementation / deliverable checklist

- [ ] Require touched-zone origin, contiguous prior-close crossing and frozen E clearance; resolve shallow crossings as unconfirmed.
- [ ] Allow retests starting with the next completed candle through bar 15 on 1m or bar 6 on 5m; prohibit a same-candle retest and enforce 1-point proximity and <=50% penetration.
- [ ] Apply all terminal checks in frozen priority order and retain secondary failure evidence.
- [ ] Consume the first qualifying retest once, preserve blocked/expired/invalidated records and prevent retest duplication as an independent rejection.

## Required tests and verification

- [ ] Verify same-candle prohibition, next-candle acceptance, freshness and no drift-based watch.
- [ ] Check bars 15/6 versus 16/7, exact half-width penetration versus deeper wick, symmetric close failure and gap invalidation.
- [ ] Exercise simultaneous terminal reasons and confirm one candidate opportunity per watch with original deadlines.

Run focused tests for this task and relevant integration/regression checks. Record actual commands, versions and results; an unexecuted test plan is not a passing result. Follow applicable repository testing and change-process instructions during implementation.

## Assigned frozen acceptance cases

- [ ] **L2-AC10** — A first-next-candle retest can qualify; the breakout candle cannot retest itself. ([source](../../layer_2_price_reactions.md)).
- [ ] **L2-AC11** — Unbuffered closes and already-cleared prior closes cannot create later drift-based breakout watches. ([source](../../layer_2_price_reactions.md)).
- [ ] **L2-AC12** — Retests on bars 15/6 can qualify; bars 16/7 cannot. Missing intervals invalidate rather than extend the watch. ([source](../../layer_2_price_reactions.md)).
- [ ] **L2-AC13** — Exactly 50% penetration is allowed; deeper wicks invalidate despite recovery. Failed-close rules mirror correctly. ([source](../../layer_2_price_reactions.md)).
- [ ] **L2-AC14** — Each watch has at most one candidate; blocked triggers are not queued; consumed records survive restart. ([source](../../layer_2_price_reactions.md)).

Shared cases require evidence from all listed owners in the traceability checklist. Do not close a shared acceptance case based only on this task's portion.

## Deliverables

Breakout/watch evaluator, terminal event evidence and symmetric scenario tests.

Implement only within the new `spx-gex-agent` application unless a separate instruction authorizes otherwise. Record actual implementation/test paths when work begins; this plan does not prescribe unreviewed module names.

## Scope limits

No queued blocked entry, refreshed expiry, deleted consumed history or standalone break entry.

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

