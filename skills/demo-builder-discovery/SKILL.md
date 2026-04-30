---
name: demo-builder-discovery
description: "Interactive discovery and segmentation for local-execution UiPath Maestro Flow demo builds. Clarifies ambiguous requests, researches the operation, reads project docs, decomposes the demo slice, and builds a Flow-oriented task matrix for agents, connector activities, Flow tool nodes, Flow control nodes, and triggers."
allowed-tools: Bash, Read, Write, Edit, Glob, Grep, WebFetch, WebSearch, AskUserQuestion
---

# Demo Builder - Discovery

Turns a use case or customer brief into a small Flow-buildable demo slice.

## Scope Disclosure

Before deep work, state the practical scope:

- Will build: discovery artifacts, Flow architecture inputs, task matrix, local Flow node assumptions, and demo-ready happy/exception paths.
- Will not build by default: API workflows, RPA workflows, Human Tasks, Case Management, Data Fabric, Coded Apps, frontends, or tenant resources that require unavailable credentials.
- Demo-grade only: use deterministic fixtures and mock-shaped adapters when live integrations are not ready.

## Clarify Before Research

Ask before researching when the prompt leaves material gaps:

1. Business goal and operation name.
2. Industry/domain and regional or regulatory variant.
3. Demo duration.
4. Known systems, connectors, tool needs, agents, documents, and folders.
5. Must-show capabilities.
6. Happy path and one exception path.
7. Agent style preference: existing, coded, low-code, or inline Flow agent.

## Reference Documentation Intake

1. Check for `docs/` at repo root and create it if missing.
2. Invite the user to drop SOPs, policy docs, API docs, sample payloads, screenshots, or connector notes into `docs/`.
3. Inventory and read provided docs before finalizing research.
4. Cite user-provided docs as `[DOC-###]`; cite web sources as `[SRC-###]`.
5. User-provided docs outrank web sources when they conflict.

## Workflow

1. Scope and clarify.
2. Research the operation using web sources and provided docs. Full mode needs at least 5 sources; fast presales mode needs at least 3.
3. Copy `templates/use-case-research.template.md` to `builds/<demo-slug>/discovery/use-case-research.md`.
4. Copy `templates/source-register.template.md` to `builds/<demo-slug>/discovery/source-register.md`.
5. Copy `templates/segment-map.template.md` to `builds/<demo-slug>/discovery/process-map.md` and describe 3-4 logical process segments.
6. Copy `templates/task-automation-matrix.template.md` to `builds/<demo-slug>/discovery/task-automation-matrix.md`.
7. Mark each task with one execution type: `AI Agent`, `Connector Activity`, `Flow Tool`, `Flow Control`, or `Trigger`.
8. Checkpoint with the user when the resource mix, connector availability, or non-local resource assumptions are material to the build.

## Completion Criteria

- Scope disclosure delivered.
- Ambiguity resolved before build planning.
- Reference docs inventoried or explicitly skipped.
- Source register written.
- Process map covers 3-4 segments with entry and exit outcomes.
- Task matrix covers happy path plus one exception path.
- Every `AG-*`, `CONN-*`, `TOOL-*`, and `CTRL-*` candidate has enough detail for Flow architecture.

## Hand-Off

Return to the planner, which invokes `demo-builder-flow` for Flow architecture and resource discovery.

## References

- `references/research-and-citation-rules.md`
- `references/mapping-conventions.md`

## Templates

- `templates/use-case-research.template.md`
- `templates/source-register.template.md`
- `templates/segment-map.template.md`
- `templates/task-automation-matrix.template.md`
