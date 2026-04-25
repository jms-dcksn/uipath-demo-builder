# Delivery Workflow — Expanded

The `SKILL.md` summary is the phase outline. This doc expands each phase with inputs, outputs, and completion criteria. Case Management is the only orchestration model for this skill set.

## Build directory

Every phase reads and writes under `builds/<demo-slug>/` per the layout in `SKILL.md` -> "Build directory convention". Sub-agents (`data-modeler`, `case-designer`, `agent-builder`, `frontend-builder`) must be given this path on dispatch. Do not let sub-agents re-derive upstream artifacts. If a prior phase's output is missing, the architect backfills it before dispatching.

At build start, copy `templates/build-manifest.template.md` to `builds/<demo-slug>/manifest.md`. Exit step for every phase: append to `manifest.md` the phase status (`DONE`/`SKIPPED`/`FAILED`), artifact paths produced, and any open issues. Use the Edit tool, never overwrite — multiple writers preserve order.

## Phase 0 — Preflight (architect, < 30s)

Run and record on `manifest.md`:
- `uip --version`
- `uip login status --output json`
- `uip maestro case --help` (must list registry/cases/stages/tasks/edges/var/sla/validate)
- `uip codedagent --help`
- `uip solution --help`

If any required surface is absent, abort the build and surface a single blocker message to the user. Do NOT proceed to discovery. The user must upgrade the CLI or switch machines first. Record CLI version and login state on `manifest.md` regardless of outcome.

Exit step: append `DONE` or `FAILED` to `manifest.md`, with CLI version, login state, missing surfaces, and the decision to proceed or abort.

## Phase 1 — Discovery

If the user provided ideal inputs, delegate directly to **`demo-builder-discovery`**. That skill runs scope disclosure, clarifying questions, `docs/` intake, web research (per `research-and-citation-rules.md`), segmentation (3-4 `SEG-##`), and the task matrix (`T-###` with execution type).

If the user provided only an account name:
1. Research the account (industry, lines of business, known automation initiatives, recent news).
2. Draft 2-3 candidate use cases, each with: title, one-paragraph business goal, industry/domain, likely automation mix, demo hook.
3. Present via `AskUserQuestion` — let the user pick one or supply their own.
4. Then have `demo-builder-discovery` fill its `use-case-brief.template.md` and continue.

Completion criteria for this phase:
- ≥ 5 sources per use case; ≥ 2 describing real operational workflows; ≥ 1 on compliance/risk if applicable.
- Every requirement (`BR-*`) maps to at least one task and one field.
- Segment entry/exit outcomes fit a Case Management lifecycle.
- User approved the segment map + task matrix before hand-off.

Exit step: append `DONE`/`SKIPPED`/`FAILED` to `manifest.md`, with discovery artifact paths, source count, user approval state, and open issues.

## Phase 2 — Initial case data model

Dispatch the **`data-modeler`** sub-agent. It invokes `demo-builder-data-fabric` and writes to `builds/<demo-slug>/case-entity/`. This is the initial schema based on discovery output.

Exit step: append `DONE`/`SKIPPED`/`FAILED` to `manifest.md`, with schema/example paths, field count, validation result, and open schema questions.

## Phase 3a — Case Management design (sub-agent)

Dispatch the **`case-designer`** sub-agent. It invokes `demo-builder-case-management`, maps segments/tasks to stages and case tasks, **authors a stub contract for every non-agent component** (Trigger, RPA, API, IDP, Intermediate Event) with hardcoded demo I/O for happy + exception paths, and synthesizes a minimal `sdd.md`. Artifacts land in `builds/<demo-slug>/flow-model/`.

The sub-agent returns `sdd.md` and `case-management-design.md`. It MUST NOT author `caseplan.json` directly. If `sdd.md` cannot be completed, it returns an error to the architect and writes no `flow-model/caseplan.json`.

**Critical**: the sub-agent's report contains a handoff stub list. The architect MUST:
- Surface the list to the user for review (what they will eventually build for real).
- Pass the stub I/O shapes to `agent-builder` and `frontend-builder` as the canonical interfaces. Agents consume stub outputs via tool wrappers with identical shapes; the frontend renders stub outputs as normal case entity fields.

Exit step: append `DONE`/`SKIPPED`/`FAILED` to `manifest.md`, with design and `sdd.md` paths, user approval state, missing entity fields, and open issues.

## Phase 3b — caseplan.json generation (architect, main thread)

Architect invokes `uipath-case-management` skill via the Skill tool, passing `builds/<demo-slug>/flow-model/sdd.md` as input. Skill produces `tasks.md` (requires user approval), then `caseplan.json`.

Only the architect has the Skill tool needed for this delegation. Do not ask `case-designer` or any sub-agent to invoke `uipath-case-management`.

Exit step: append `DONE`/`SKIPPED`/`FAILED` to `manifest.md`, with `tasks.md` and `caseplan.json` paths, approval status, unresolved skeleton task markers, validation result, and open issues.

## Phase 4 — Case data model reconciliation

Re-dispatch the **`data-modeler`** sub-agent in reconciliation mode. It compares the Phase 3 case design and stub contracts against `case-entity.schema.json`.

Rules:
- Every persisted output from a trigger, RPA/API/IDP stub, human task, or agent must map to an existing case entity field.
- If a needed field is missing, add it to the schema and example records.
- If an output is not persisted, mark it as non-persistent in `data-fabric-modeling-notes.md`.
- Revalidate JSON after changes.

Exit step: append `DONE`/`SKIPPED`/`FAILED` to `manifest.md`, with reconciled schema/example paths, notes path, validation result, and open issues.

## Phase 5 — Agents (HARD CHECKLIST)

Before dispatching:
- [ ] Enumerate every `AG-*` ID from the task matrix.
- [ ] Confirm each has a 1:1 mapping to a planned project directory.

Dispatch:
- [ ] Send ONE assistant message containing N parallel `Agent` tool calls (one per `AG-*`). Sequential dispatch is a build error.
- [ ] If you find yourself about to send a second turn before all `AG-*` are dispatched: STOP. Re-issue as a single parallel turn.

After dispatch:
- [ ] Each report read once. Mismatches between agent outputs and example records -> Phase 5.5 fixture consistency check.

For each `AG-*` identified in the task matrix, dispatch an **`agent-builder`** sub-agent. The agents are independent by design. Each instance:
1. Writes a design brief (role, boundaries, prompt strategy, tool policy, escalation path, output contract) to `builds/<demo-slug>/agents/<AG-id>/agent-build-spec.md`. Template lives in `demo-builder-agents/templates/agent-build-spec.template.md`.
2. Invokes `demo-builder-agents` to scaffold and build under `builds/<demo-slug>/agents/<AG-id>/`. The skill asks coded vs low-code and Studio Web vs Orchestrator deploy.

Rules:
- 1:1 mapping between each `AG-*` and a scaffolded project.
- Never multiplex multiple role/system prompts inside one runtime.
- If user provided Context Grounding: wire `ContextGroundingRetriever` with both `index_name` and `folder_path`.
- If user provided MCP URL: integrate streamable HTTP MCP tools.
- If integrations are unavailable: implement mock tools with deterministic outputs for the demo paths. Keep mock/real interfaces identical.
- Every coded agent must have `evaluations/eval-sets/smoke-test.json`.

Exit step: append `DONE`/`SKIPPED`/`FAILED` to `manifest.md`, with one row per `AG-*`, project path, local run status, `uip codedagent eval` status or skipped reason, and open issues.

## Phase 5.5 — Fixture consistency check (architect)

For each example record in `case-entity/case-entity.example.json`:
1. Compute the deterministic output of every `AG-*` against that record's inputs (read each agent's policy catalog or fixture map).
2. Compare against the record's persisted agent-output fields (`decision.*`, `conditions[]`, `income.monthlyVerifiedUSD`, etc.).
3. On mismatch: patch the example record to match the agent OR escalate to user if the agent contract is wrong.

Output: a one-page consistency report in `case-entity/fixture-consistency.md` listing every checked field and resolution. Required before Phase 6.

Exit step: append `DONE`/`SKIPPED`/`FAILED` to `manifest.md`, with fixture consistency report path, fields checked, patched record paths, escalations, and open issues.

## Phase 6 — Frontend

Dispatch the **`frontend-builder`** sub-agent. It invokes `demo-builder-frontend` and produces a UiPath Coded Web App using Vite + React + TypeScript and the UiPath TypeScript SDK, with dashboard + case detail pages and an explicit UI data contract, under `builds/<demo-slug>/frontend/`.

Exit step: append `DONE`/`SKIPPED`/`FAILED` to `manifest.md`, with frontend path, route summary, `npm run build` status, visual verification status, and open issues.

## Phase 6.5 — Frontend ↔ schema reconcile

After `frontend-builder` reports, diff the TypeScript types in `frontend/src/types/` against `case-entity.schema.json`. Any field in `fixtures.ts` not present in the schema -> re-dispatch `data-modeler` in reconciliation mode to add. Required before Phase 7.

Exit step: append `DONE`/`SKIPPED`/`FAILED` to `manifest.md`, with checked type/fixture paths, schema additions requested or confirmed absent, reconciliation output paths, and open issues.

## Phase 7 — Manual completion checklist and optional production-spec expansion

Copy `templates/manual-completion-checklist.template.md` to `builds/<demo-slug>/manual-completion-checklist.md` and fill it. This is required for every build.

The checklist must include:
- Data Fabric entity import and fixture records.
- Case Management project validation, publish, deploy, and unresolved skeleton task follow-up.
- Coded agent Studio Web project IDs, push/deploy status, Context Grounding indexes, MCP URLs, and auth requirements.
- Coded Web App External Application/OAuth setup, scopes, folder keys, build/package/publish/deploy status.
- RPA, API, IDP, trigger, and intermediate-event components that remain handoff items.

The **stub contracts from Phase 3** are already the demo handoff: each non-agent component has I/O shapes, hardcoded demo values, and a mock hint that lets the user stand up a pass-through workflow in minutes. Production-spec expansion is only run when the user explicitly asks for production-grade specs on top of the demo stubs.

When run:
- Expand each `CMP-*` from the Phase 3 stub list into a full `templates/component-specifications.template.md` row with selectors (RPA), extraction fields + confidence thresholds (IDP), endpoint + error codes (API), retry/idempotency, audit requirements, SLA.
- Flag every component as a handoff item in `templates/delivery-backlog.template.md`.

Skip entirely for demo-only builds — the stub contracts already carry the essential handoff info (input schema, output schema, demo values, real-build note).

Exit step: append `DONE`/`SKIPPED`/`FAILED` to `manifest.md`, with checklist path, handoff items counted by component type, skipped production-spec rationale, and open issues.

## Phase 8 — Demo script

Delegate to **`demo-builder-script`**. Produces 3-4 key messages, each mapped to 2-3 implemented visuals from the frontend and manifest, with narrator line + operator action + on-screen proof per beat. Opens with a value statement; closes with business impact.

Exit step: append `DONE`/`SKIPPED`/`FAILED` to `manifest.md`, with demo script path, message/visual counts, dry-run notes, and open issues.

## Cross-phase completion criteria

- Every `BR-*` traces to at least one `T-*` and one field.
- Every `SEG-*` contains at least one `T-*`.
- Every UI action maps to a payload contract and business outcome.
- Every orchestration decision point references a rule ID (`R-*`).
- Every `AG-*` has a scaffolded project and a filled agent build spec.
- Every non-agent / non-human task has a `CMP-*` stub contract with concrete I/O schema and hardcoded demo values (happy + exception).
- Every stub output field that persists maps to a field in the case entity schema.
- Every example record matches deterministic agent outputs, with `case-entity/fixture-consistency.md` written.
- Frontend fixtures/types contain no fields absent from `case-entity.schema.json` unless `data-modeler` reconciled them.
- `manifest.md` lists all artifacts and validation status.
- `manual-completion-checklist.md` lists every tenant-side step the user must complete.
- Unknowns are marked `TBD` with owner/date — never silently inferred.
