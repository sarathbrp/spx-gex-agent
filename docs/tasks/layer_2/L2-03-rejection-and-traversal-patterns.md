# L2-03 — Rejection geometry and traversal observations

**Status:** NOT_STARTED  
**Group:** layer_2  
**Task type:** Implementation  
**Owner:** Unassigned  
**Created:** 2026-10-29  
**Specification baseline:** Frozen v1.0

[Master checklist](../../TASKS.md) · [Requirements traceability](../TRACEABILITY.md)

## Objective

Emit symmetric completed-candle rejection evidence without next-bar confirmation.

## Completion checklist

- [ ] Implementation or deliverable completed
- [ ] Tests / verification checks passed
- [ ] Acceptance criteria verified
- [ ] Reviewed and evidence recorded
- [ ] Complete — master index updated

Every box starts unchecked. Creating this plan is not implementation or testing. Update this task and its master-index entry together; do not mark Complete until required checks and acceptance evidence are recorded. A verification task completes its stated evidence deliverable rather than claiming application code was implemented.

## Authoritative requirements

- [Layer 2 specification](../../layer_2_price_reactions.md) — sections 7, 8, 13.

The linked frozen specifications take precedence over this task summary. Do not silently introduce new strategy rules. Record any material unresolved input or implementation decision below before dependent work proceeds.

## Dependencies

- [ ] [L2-02 — Interaction episodes, contact ownership and re-arming](../layer_2/L2-02-episodes-contact-and-rearming.md) completed, or an explicitly documented interface prerequisite is available.

Dependencies describe prerequisites, not permission to change another task's scope. Tests may use agreed fixtures/interfaces before full integration; acceptance still requires the actual dependencies and relevant integration checks.

## Implementation / deliverable checklist

- [ ] Implement support/resistance predicates using locked approach, current boundary contact and recovery.
- [ ] Require range >=0.50, directional wick >=0.40 and exact directional close fraction >=2/3.
- [ ] Classify full-zone recovery separately and preserve unqualified returns with failed-condition evidence.
- [ ] Return immutable pattern evidence without directly creating fills or outcome-based confirmation.

## Required tests and verification

- [ ] Check inclusive range/wick/close endpoints and values immediately below them.
- [ ] Verify the documented 30.9% upper-wick example fails; mirror every CALL fixture into PUT behavior.
- [ ] Test multi-bar origin persistence, no borrowed prior wick, far-boundary equality and full traversal exclusions.

Run focused tests for this task and relevant integration/regression checks. Record actual commands, versions and results; an unexecuted test plan is not a passing result. Follow applicable repository testing and change-process instructions during implementation.

## Assigned frozen acceptance cases

- [ ] **L2-AC04** — Range exactly 0.50, wick exactly 0.40 and close placement exactly 2/3 pass; values below fail. ([source](../../layer_2_price_reactions.md)).
- [ ] **L2-AC05** — The 30.9% upper-wick example produces no rejection candidate. ([source](../../layer_2_price_reactions.md)).
- [ ] **L2-AC06** — Symmetric full-zone recoveries are excluded from ordinary rejections. ([source](../../layer_2_price_reactions.md)).

Shared cases require evidence from all listed owners in the traceability checklist. Do not close a shared acceptance case based only on this task's portion.

## Deliverables

Rejection/traversal evaluator with geometric proof fixtures.

Implement only within the new `spx-gex-agent` application unless a separate instruction authorizes otherwise. Record actual implementation/test paths when work begins; this plan does not prescribe unreviewed module names.

## Scope limits

No next-candle requirement, invented liquidity explanation or direct candidate publication outside eligibility.

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

