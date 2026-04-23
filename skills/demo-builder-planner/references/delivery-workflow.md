# Delivery Workflow — Expanded

The `SKILL.md` summary is the phase outline. This doc expands each phase with inputs, outputs, and completion criteria. Case Management is the orchestration model for this skill set — BPMN is out of scope.

## Build directory

Every phase reads and writes under `builds/<demo-slug>/` per the layout in `SKILL.md` → "Build directory convention". Sub-agents (`data-modeler`, `case-designer`, `agent-builder`, `frontend-builder`) must be given this path on dispatch. Do not let sub-agents re-derive upstream artifacts — if a prior phase's output is missing, the architect backfills it before dispatching.

## Phase 0 — Gather inputs

**If user provided ideal inputs:** proceed to Phase 1.

**If user provided only an account name:**
1. Research the account (industry, lines of business, known automation initiatives, recent news).
2. Draft 2-3 candidate use cases, each with: title, one-paragraph business goal, industry/domain, likely automation mix, demo hook.
3. Present via `AskUserQuestion` — let the user pick one or supply their own.
4. Then have `demo-builder-discovery` fill its `use-case-brief.template.md` and continue.

## Phase 1 — Discovery

Delegate to **`demo-builder-discovery`**. That skill runs scope disclosure, clarifying questions, `docs/` intake, web research (per `research-and-citation-rules.md`), segmentation (3-4 `SEG-##`), and the task matrix (`T-###` with execution type).

Completion criteria for this phase:
- ≥ 5 sources per use case; ≥ 2 describing real operational workflows; ≥ 1 on compliance/risk if applicable.
- Every requirement (`BR-*`) maps to at least one task and one field.
- Segment entry/exit outcomes fit a Case Management lifecycle.
- User approved the segment map + task matrix before hand-off.

## Phase 2 — Case data model

Dispatch the **`data-modeler`** sub-agent. It invokes `demo-builder-data-fabric` and writes to `builds/<demo-slug>/case-entity/`. Lifecycle/state field must align with the Case Management stages produced in Phase 3.

## Phase 3 — Case Management design (stub contracts are emitted here)

Dispatch the **`case-designer`** sub-agent. It invokes `demo-builder-case-management`, maps segments/tasks to stages and case tasks, **authors a stub contract for every non-agent component** (Trigger, RPA, API, IDP, Intermediate Event) with hardcoded demo I/O for happy + exception paths, synthesizes a minimal `sdd.md`, and delegates to `uipath-case-management` for `caseplan.json` generation. Artifacts land in `builds/<demo-slug>/flow-model/`.

**Critical**: the sub-agent's report contains a handoff stub list. The architect MUST:
- Surface the list to the user for review (what they will eventually build for real).
- Pass the stub I/O shapes to `agent-builder` (Phase 4) and `frontend-builder` (Phase 5) as the canonical interfaces. Agents consume stub outputs via tool wrappers with identical shapes; the frontend renders stub outputs as normal case entity fields.

## Phase 4 — Agents

For each `AG-*` identified in the task matrix, dispatch an **`agent-builder`** sub-agent. **All `AG-*` builds must be spawned as parallel `Agent` tool calls in a single assistant turn** — do not dispatch one, wait for its report, then dispatch the next. The agents are independent by design; sequential dispatch loses the core benefit of sub-agenting this phase. Each instance:
1. Writes a design brief (role, boundaries, prompt strategy, tool policy, escalation path, output contract) to `builds/<demo-slug>/agents/<AG-id>/agent-build-spec.md`. Template lives in `demo-builder-agents/templates/agent-build-spec.template.md`.
2. Invokes `demo-builder-agents` to scaffold and build under `builds/<demo-slug>/agents/<AG-id>/`. The skill asks coded vs low-code and Studio Web vs Orchestrator deploy.

Rules:
- 1:1 mapping between each `AG-*` and a scaffolded project.
- Never multiplex multiple role/system prompts inside one runtime.
- If user provided Context Grounding: wire `ContextGroundingRetriever` with both `index_name` and `folder_path`.
- If user provided MCP URL: integrate streamable HTTP MCP tools.
- If integrations are unavailable: implement mock tools with deterministic outputs for the demo paths. Keep mock/real interfaces identical.

## Phase 5 — Frontend

Dispatch the **`frontend-builder`** sub-agent. It invokes `demo-builder-frontend` and produces a Vite + React + TypeScript app using the UiPath TypeScript SDK, with dashboard + case detail pages and an explicit UI data contract, under `builds/<demo-slug>/frontend/`.

## Phase 6 — Proprietary handoff (optional, production-spec expansion)

The **stub contracts from Phase 3** are already the demo handoff: each non-agent component has I/O shapes, hardcoded demo values, and a mock hint that lets the user stand up a pass-through workflow in minutes. Phase 6 is only run when the user explicitly asks for production-grade specs on top of the demo stubs.

When run:
- Expand each `CMP-*` from the Phase 3 stub list into a full `templates/component-specifications.template.md` row with selectors (RPA), extraction fields + confidence thresholds (IDP), endpoint + error codes (API), retry/idempotency, audit requirements, SLA.
- Flag every component as a handoff item in `templates/delivery-backlog.template.md`.

Skip entirely for demo-only builds — the stub contracts already carry the essential handoff info (input schema, output schema, demo values, real-build note).

## Phase 7 — Demo script

Delegate to **`demo-builder-script`**. Produces 3-4 key messages, each mapped to 2-3 visuals, with narrator line + operator action + on-screen proof per beat. Opens with a value statement; closes with business impact.

## Cross-phase completion criteria

- Every `BR-*` traces to at least one `T-*` and one field.
- Every `SEG-*` contains at least one `T-*`.
- Every UI action maps to a payload contract and business outcome.
- Every orchestration decision point references a rule ID (`R-*`).
- Every `AG-*` has a scaffolded project and a filled agent build spec.
- Every non-agent / non-human task has a `CMP-*` stub contract with concrete I/O schema and hardcoded demo values (happy + exception).
- Every stub output field that persists maps to a field in the case entity schema.
- Unknowns are marked `TBD` with owner/date — never silently inferred.
