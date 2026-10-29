# Agent Instructions — spx-gex-agent

## Project boundary

This folder is an independent research application. Keep its package, entry points, configuration, dependencies, SQLite store, logs, tests and release history separate from `spx-agent` / `spx_orb`.

- Do not add runtime imports that depend on `spx_orb`, modify its files/data, or rely on its directory being on `PYTHONPATH`.
- Before reusing existing code, complete the provider/reuse audit in F02. Port only the specific, understood behavior needed here, give it a native interface, preserve useful provenance, and verify it against this app's contract. Do not copy the whole application.
- Keep one SQLite writer and read-only viewers as specified in [Persistent State Machine and Storage Schema](docs/persistent_state_and_storage.md).
- Treat Layers 3 and 4 as observations. They do not veto or delay an eligible Layer 2 candidate.
- Broker access, order placement, portfolio/fill inference, live execution and unapproved score rules are outside the current scope.

## Source of truth and document map

Read [ARCHITECTURE.md](ARCHITECTURE.md) for the design index. Use the following document for the matching branch of work:

- **Before changing a strategy rule or signal calculation:** read the relevant frozen layer specification. [Layer 1](docs/layer_1_zone_discovery.md), [Layer 2](docs/layer_2_price_reactions.md), [Layer 3](docs/layer_3_environmental_context.md), and [Layer 4](docs/layer_4_multitimeframe_alignment.md) are authoritative for their scope.
- **Before changing persistence, recovery, identity, transactions or database access:** read [Persistent State Machine and Storage Schema](docs/persistent_state_and_storage.md).
- **Before starting implementation:** select a task from [TASKS.md](docs/TASKS.md), read its task file under `docs/tasks/<group>/`, inspect its dependencies and open its assigned cases in [TRACEABILITY.md](docs/tasks/TRACEABILITY.md).
- **When changing a requirement:** edit its authoritative frozen specification only after the user agrees to the change; increment its version and record the decision. Then reconcile dependent tasks, acceptance mappings and the architecture index. Never encode an unapproved reinterpretation only in code or a task summary.

Document responsibilities:

| Location | Responsibility |
|---|---|
| `AGENTS.md` | Durable project boundaries and work rules |
| `ARCHITECTURE.md` | Entry point linking design specifications and tracking documents |
| `docs/layer_*.md` | Frozen strategy contracts, one per layer |
| `docs/persistent_state_and_storage.md` | Frozen storage, concurrency and recovery contract |
| `docs/TASKS.md` | Task order, dependencies and master progress checklists |
| `docs/tasks/<group>/<ID>-<slug>.md` | One task's scope, dependencies, deliverables, tests and evidence |
| `docs/tasks/TRACEABILITY.md` | Mapping from each frozen requirement and acceptance case to owning tasks and evidence |
| `docs/decisions/` | Optional concise records for lasting implementation choices not already governed by a frozen specification |

Keep one authoritative home for each rule. Task files summarize and link to requirements; they do not duplicate the full strategy specification. Use `docs/decisions/` only when a durable implementation decision adds useful context and is not already represented by code/configuration or a source specification.

## Task workflow

For each implementation task:

1. Identify the task ID. Read its complete task file, linked specification sections, acceptance cases, and dependencies. Check the current repository state before editing.
2. Restate the requested behavior and scope, then present the implementation approach, a materially better alternative if one exists, and risks. Ask a short question when a required choice is genuinely unresolved. Wait for agreement unless the user explicitly said to proceed without that step.
3. Make the smallest coherent change that satisfies the task. Keep unrelated refactors and later-task features out of the change.
4. Follow the task's test and verification checklist. Record commands and actual results. Never mark a check passed because it is planned, because related code exists, or because a fixture does not cover the real input condition.
5. Before calling the task complete, update its task file, the matching master checklist entry and counts in `docs/TASKS.md`, and each acceptance item in `docs/tasks/TRACEABILITY.md` for which all owners have supplied evidence. Add links to test output, reports or provider/session evidence where available.
6. Report changed files, observed verification results, unchecked cases, and remaining blockers. Do not claim live observation or predictive validation from unit/fixture results.

When work stops partway through, leave the task `IN_PROGRESS` or `BLOCKED`, record what exists and what remains, and leave incomplete checks unchecked. Do not advance downstream tasks by treating an unfinished dependency as complete.

## Documentation updates after every task

At task close, make the documentation state match the actual work:

- **Task file:** set status; check only completed implementation/deliverable, verification, acceptance, review and completion items. Fill in paths, commands, results, fixture/config versions, case evidence, review notes and unresolved blockers. Keep unrun tests marked not run.
- **Master checklist:** update that task's completion stages and the per-group/total complete counts. Keep all other task rows unchanged. If task order or dependency truth changed, update the relevant links and explanation.
- **Traceability:** close an acceptance checkbox only after every listed owner has evidence. Add precise evidence links and retain shared ownership. Requirement-section mappings change only when the owning work changes.
- **Frozen specification:** leave it unchanged for an implementation-only task. If the user approves a requirement change, update the spec/version and its acceptance cases, then reconcile every dependent task and mapping in the same documentation change.
- **Architecture index:** update when a document, frozen status, project boundary or major module/data flow is added or changed. Keep it an index rather than copying task details into it.
- **Decision record:** add a short record under `docs/decisions/` only for an agreed lasting implementation decision that future work needs and cannot be located in the current specifications or source configuration. Link it from the architecture index or task; avoid parallel duplicate specs.

Task-document creation is planning only. It does not satisfy implementation, testing, review, acceptance, provider verification or live-observation checkboxes.

## Coding rules

- **Deterministic core:** strategy/state transitions consume explicit timestamped inputs and configuration, return typed results/events, and make no network, database, console or clock calls internally. Keep provider, clock, persistence and display effects in adapters/orchestration.
- **State ownership:** each stateful module owns a clear part of the lifecycle. Prefer cohesive modules and simple interfaces; use pure functions for calculations and stateful objects where a lifecycle must persist. Avoid broad shared mutable state and arbitrary SQL outside the storage module.
- **Causality:** preserve source, availability, decision and event-order clocks. Every decision reads only data available by its recorded cutoff. Never backfill, forward-fill, infer an intrabar path from OHLC, or turn unavailable/malformed data into zero, neutral context or a signal.
- **Contracts and types:** use explicit domain types/enums for statuses, identities, directions and timeframes; validate external/provider data at ingress. Use timezone-aware timestamps, canonical option contract keys, finite numeric values and explicit units. Keep price identity and zone-boundary comparison consistent with the storage contract.
- **Persistence:** use the storage interface and atomic transition protocol. Preserve immutable source revisions and events; make retries idempotent; retain missingness and reasons. Commit before publishing a candidate. Never expose future outcome tables to decision logic.
- **Errors and observability:** surface actionable provider, parse, health, lock and database failures with source/context. Preserve the failed operation and affected interval. Do not swallow exceptions, silently retry forever, or hide gaps behind stale healthy state.
- **Dependencies:** prefer the standard library when it meets the contract. Add a package only for a concrete task need; record why and verify platform/runtime compatibility. Keep credentials out of source, logs, fixtures and database payloads.
- **Scope and size:** implement only the active task and its required interfaces. Split source and test files by responsibility before they become difficult to review; target fewer than 300 lines per file where practical. Do not impose a coverage percentage until F01 records an agreed project standard.
- **Tests and evidence:** task acceptance cases define required behavior. Add focused tests with the feature when the task or user request includes test work; use deterministic fixtures for boundary, missing-data, restart and causality cases. Record actual test commands/results and never report unrun checks as passing.
- **Comments:** explain a non-obvious invariant or reason, not what the next line already says. Update comments when the behavior changes.

## Review and completion

Before handing off, inspect the full diff for task scope, frozen-contract consistency, accidental changes to `spx_orb`, generated files, secrets and stale documentation. Check every changed local Markdown link. Run only verification required by the active task or explicitly requested by the user; report anything not run.

A task is complete only when its deliverable exists, required verification has actual recorded results, its assigned acceptance evidence is satisfied, material findings are resolved or explicitly carried forward, and the task/index/traceability records agree. Live observation and research performance claims require their own evidence and remain open until observed.
