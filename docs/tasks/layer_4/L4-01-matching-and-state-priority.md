# L4-01 — Spatial/timeframe matching and five-state resolution

**Status:** NOT_STARTED  
**Group:** layer_4  
**Task type:** Implementation  
**Owner:** Unassigned  
**Created:** 2026-10-29  
**Specification baseline:** Frozen v1.0

[Master checklist](../../TASKS.md) · [Requirements traceability](../TRACEABILITY.md)

## Objective

Compare completed counterpart candidates deterministically without changing the primary candidate.

## Completion checklist

- [ ] Implementation or deliverable completed
- [ ] Tests / verification checks passed
- [ ] Acceptance criteria verified
- [ ] Reviewed and evidence recorded
- [ ] Complete — master index updated

Every box starts unchecked. Creating this plan is not implementation or testing. Update this task and its master-index entry together; do not mark Complete until required checks and acceptance evidence are recorded. A verification task completes its stated evidence deliverable rather than claiming application code was implemented.

## Authoritative requirements

- [Layer 4 specification](../../layer_4_multitimeframe_alignment.md) — sections 1, 2, 3, 4, 5, 6, 7.

The linked frozen specifications take precedence over this task summary. Do not silently introduce new strategy rules. Record any material unresolved input or implementation decision below before dependent work proceeds.

## Dependencies

- [ ] [L2-05 — Candidate eligibility, phase gates and durable publication](../layer_2/L2-05-candidate-eligibility-and-publication.md) completed, or an explicitly documented interface prerequisite is available.

Dependencies describe prerequisites, not permission to change another task's scope. Tests may use agreed fixtures/interfaces before full integration; acceptance still requires the actual dependencies and relevant integration checks.

## Implementation / deliverable checklist

- [ ] Match session/universe/configuration, exact member set, each-bound tolerance <=0.01, the other timeframe and both 300-second clock limits plus the event cursor.
- [ ] Choose the counterpart interval covering the primary confirmation instant before its close boundary.
- [ ] Implement UNAVAILABLE, CONFLICT, AGREEMENT, PENDING_CLOSE, NO_MATCH priority and all-match preservation.
- [ ] Choose deterministic representative and family labels; isolate forming shape, partial overlap and missing evaluation from valid matching.

## Required tests and verification

- [ ] Cover exact 0.01-point and 300-second endpoints, delayed old candidates and same-clock event ordering.
- [ ] Exercise shared-close completed/no-match versus pending-delivery, completed earlier agreement while another candle forms.
- [ ] Test simultaneous same/opposite matches, family types, representative ties and null spatial match when no counterpart exists.

Run focused tests for this task and relevant integration/regression checks. Record actual commands, versions and results; an unexecuted test plan is not a passing result. Follow applicable repository testing and change-process instructions during implementation.

## Assigned frozen acceptance cases

- [ ] **L4-AC01** — Every eligible Layer 2 candidate survives all five comparison states and all Layer 4 errors. ([source](../../layer_4_multitimeframe_alignment.md)).
- [ ] **L4-AC02** — Same underlying/session/universe/configuration and exact member sets are required; partial overlap cannot establish agreement. ([source](../../layer_4_multitimeframe_alignment.md)).
- [ ] **L4-AC03** — Each bound at 0.01-point difference passes tolerance; larger differences fail without transitive geometry merging. ([source](../../layer_4_multitimeframe_alignment.md)).
- [ ] **L4-AC04** — Both availability and completion windows are enforced; exact 300-second boundaries pass and greater ages fail. ([source](../../layer_4_multitimeframe_alignment.md)).
- [ ] **L4-AC06** — Missing required data wins summary priority but does not delete known evidence. ([source](../../layer_4_multitimeframe_alignment.md)).
- [ ] **L4-AC07** — When same- and opposite-side matches coexist, conflict wins the summary, all matches remain stored, and the representative tie-break is deterministic. ([source](../../layer_4_multitimeframe_alignment.md)).
- [ ] **L4-AC08** — Homogeneous and heterogeneous agreement are distinguished; absence of a counterpart never sets spatial match to true. ([source](../../layer_4_multitimeframe_alignment.md)).
- [ ] **L4-AC09** — The relevant interval ending at a shared boundary is selected instead of the newly forming interval; complete no-candidate evaluation produces NO_MATCH. ([source](../../layer_4_multitimeframe_alignment.md)).
- [ ] **L4-AC10** — Completed recent agreement outranks pending status; forming wick shape never creates agreement/conflict or a veto. ([source](../../layer_4_multitimeframe_alignment.md)).

Shared cases require evidence from all listed owners in the traceability checklist. Do not close a shared acceptance case based only on this task's portion.

## Deliverables

Pure Layer 4 comparison evaluator with clock/priority fixtures.

Implement only within the new `spx-gex-agent` application unless a separate instruction authorizes otherwise. Record actual implementation/test paths when work begins; this plan does not prescribe unreviewed module names.

## Scope limits

No provisional-wick matching, bonus, veto, cancellation or independent-confirmation claim.

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

