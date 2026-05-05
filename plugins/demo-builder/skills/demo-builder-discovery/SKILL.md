---
name: demo-builder-discovery
description: "Interactive discovery and segmentation for local-execution UiPath Maestro Flow demo builds. Clarifies ambiguous requests, researches the operation, reads project docs, decomposes the demo slice, researches mock system payloads, and builds a Flow-oriented task matrix for agents, mock API workflows, connector activities, Flow tool nodes, Flow control nodes, and triggers."
allowed-tools: Bash, Read, Write, Edit, Glob, Grep, WebFetch, WebSearch, AskUserQuestion
---

# Demo Builder - Discovery

Turns a use case or customer brief into a small Flow-buildable demo slice.

## Scope Disclosure

Before deep work, state the practical scope:

- Will build: discovery artifacts, Flow architecture inputs, task matrix, local Flow node assumptions, and demo-ready happy/exception paths.
- Will build when system interactions are part of the demo: deterministic mock API workflow candidates, payload research, and field maps.
- Will build when live Integration Service actions are part of the demo: connector activity candidates, connection prerequisites, and user-visible outputs.
- Will not build by default: live external API calls, RPA workflows, Human Tasks, Case Management, Data Fabric, Coded Apps, frontends, or tenant resources that require unavailable credentials.
- Demo-grade only: use deterministic fixtures, mock API workflow payloads, and mock-shaped adapters when live integrations are not ready.

## Clarify Before Research

Ask before researching when the prompt leaves material gaps:

1. Business goal and operation name.
2. Industry/domain and regional or regulatory variant.
3. Demo duration.
4. Known systems, connectors, tool needs, agents, documents, and folders.
5. Must-show capabilities.
6. Happy path and one exception path.
7. Agent style preference: existing, coded, low-code, or inline Flow agent.
8. Whether named system choices are fixed or demo-builder may select credible mocks, such as Salesforce vs ServiceNow or Guidewire vs Duck Creek.

## Reference Documentation Intake

1. Check for `docs/` at repo root and create it if missing.
2. Invite the user to drop SOPs, policy docs, API docs, sample payloads, screenshots, or connector notes into `docs/`.
3. Inventory and read provided docs before finalizing research.
4. Cite user-provided docs as `[DOC-###]`; cite web sources as `[SRC-###]`.
5. User-provided docs outrank web sources when they conflict.

## Mock API Payload Research

When the demo includes system-of-record or third-party system interactions, payload research is mandatory before Flow architecture.

Rules:

- Prefer user-provided API docs, sample payloads, screenshots, connector notes, and SOPs over web research.
- Use public API docs, sample payloads, connector docs, domain implementation guides, or credible vendor examples when user docs are unavailable.
- Select named systems instead of generic categories, such as Salesforce, ServiceNow, SAP, Epic, Guidewire, Fiserv, Workday, or Oracle.
- Keep mock payloads small enough to explain in a demo while including identity fields, decision fields, and narrative fields.
- Never use real customer data, patient data, account numbers, credentials, endpoints, or secrets.
- Mark every selected payload field as `demo-visible`, `agent-input`, `condition-input`, `connector-input`, or `end-output`.

## Connector Activity Routing

When a task needs a visible live system action, prefer `Connector Activity` if UiPath Integration Service has a curated activity and the demo can use an existing connection. Capture the service, connector key if known, activity intent, required folder or connection, and expected user-visible output.

Use `Mock API Workflow` instead when the task is deterministic system context, repeatable demo data, or a system-of-record lookup that should not depend on live tenant state. Use `Flow Tool` when local shaping, parsing, normalization, or fixture-backed enrichment is enough.

Checkpoint with the user when a named connector, folder, connection, or live write is material to the demo and cannot be inferred from local docs or current tenant discovery. Missing connector connections and unresolved required fields are blockers for connector implementation, not values to guess.

## Workflow

1. Scope and clarify.
2. Research the operation using web sources and provided docs. Full mode needs at least 5 sources; fast presales mode needs at least 3.
3. Copy `templates/use-case-research.template.md` to `builds/<demo-slug>/discovery/use-case-research.md`.
4. Copy `templates/source-register.template.md` to `builds/<demo-slug>/discovery/source-register.md`.
5. Copy `templates/segment-map.template.md` to `builds/<demo-slug>/discovery/process-map.md` and describe 3-4 logical process segments.
6. If system interactions exist, copy and fill:
   - `templates/system-interactions.template.md` to `builds/<demo-slug>/discovery/system-interactions.md`.
   - `templates/mock-api-payload-research.template.md` to `builds/<demo-slug>/discovery/mock-api-payload-research.md`.
   - `templates/payload-field-map.template.md` to `builds/<demo-slug>/discovery/payload-field-map.md`.
7. Copy `templates/task-automation-matrix.template.md` to `builds/<demo-slug>/discovery/task-automation-matrix.md`.
8. Mark each task with one execution type: `AI Agent`, `Mock API Workflow`, `Connector Activity`, `Flow Tool`, `Flow Control`, or `Trigger`.
9. Use `API-*` component IDs for mock API workflow tasks and `CONN-*` component IDs for connector activity tasks.
10. For each `API-*`, fill `Deliverable Artifact` separately from execution type. Use `Studio Web API Workflow project` for the required artifact, plus `Flow API Workflow node` or `Script placeholder invoking equivalent mock payload` for the current Flow invocation path.
11. For each `CONN-*`, fill the connector service, activity intent, connection requirement, expected output, and blocker status.
12. Checkpoint with the user when the resource mix, connector availability, named system selection, or non-local resource assumptions are material to the build.

## Completion Criteria

- Scope disclosure delivered.
- Ambiguity resolved before build planning.
- Reference docs inventoried or explicitly skipped.
- Source register written.
- Process map covers 3-4 segments with entry and exit outcomes.
- Task matrix covers happy path plus one exception path.
- System interaction research is written or explicitly marked as not needed.
- Every selected mock payload field has a documented JSON path and downstream consumer.
- Every `API-*`, `AG-*`, `CONN-*`, `TOOL-*`, and `CTRL-*` candidate has enough detail for Flow architecture.
- Every `API-*` distinguishes the required project-backed API Workflow artifact from the current Flow invocation mechanism.
- Every `CONN-*` distinguishes the live connector action from deterministic mock/API context and records connection prerequisites or blockers.

## Hand-Off

Return to the planner, which invokes `demo-builder-api-workflows` for mock API contracts and `demo-builder-flow` for Flow architecture and resource discovery.

## References

- `references/research-and-citation-rules.md`
- `references/mapping-conventions.md`

## Templates

- `templates/use-case-research.template.md`
- `templates/source-register.template.md`
- `templates/segment-map.template.md`
- `templates/task-automation-matrix.template.md`
- `templates/system-interactions.template.md`
- `templates/mock-api-payload-research.template.md`
- `templates/payload-field-map.template.md`
