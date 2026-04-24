---
name: demo-builder-planner
description: "Plan and orchestrate Case-Management-first UiPath Agentic Orchestration demo builds from a use case, industry, and requirements. Runs discovery -> segmentation -> case data model -> Case Management -> agents -> UiPath Coded Web App frontend -> demo script -> manual completion checklist. Use when the user asks to build, design, or scope a UiPath demo, or provides only a customer/account name and wants demo options."
allowed-tools: Bash, Read, Write, Edit, Glob, Grep, WebFetch, WebSearch, AskUserQuestion
user-invocable: true
---

# Demo Builder — Planner

Orchestrator skill for building UiPath Agentic Orchestration demos. Optimized for Maestro-centric solutions with Data Fabric persistence and custom front ends. **Demos, not production.** Keep code simple, prefer visual appeal and story-telling over defensive engineering.

## When to use

- User asks to build, design, scope, or propose a UiPath demo.
- User provides a use case, industry, or requirements doc.
- User provides only an account/customer name — this skill will research and propose use cases.
- User references a use-case markdown file.

## Not for

- Production-grade automation builds → use the `uipath-*` skills (`uipath-agents`, `uipath-case-management`, `uipath-rpa`, `uipath-platform`, etc.) directly.
- Running existing automations.

## Ideal vs minimum inputs

**Ideal:** use case title + one-paragraph business goal, industry/domain, demo requirements/constraints, known systems/APIs/documents.

**Minimum:** customer/account name. If that's all you have: research the account, draft ideal inputs above, present 2-3 use-case options via `AskUserQuestion`, and have the user pick one before continuing.

## Delivery workflow

Follow these phases in order. Each phase references a sibling skill or a template in `references/`. Case Management is the only orchestration model for this skill set.

1. **Discovery** — delegate to `demo-builder-discovery` for research, segmentation, and the task-to-execution-type matrix (`AI Agent` / `RPA` / `IDP` / `API` / `Human Task`). See `references/research-and-citation-rules.md` and `references/mapping-conventions.md`.
2. **Define initial case data model** in Data Fabric -> delegate to `demo-builder-data-fabric`.
3. **Case Management design** -> delegate to `demo-builder-case-management`. It synthesizes a minimal `sdd.md` and hands off to the production `uipath-case-management` skill.
4. **Reconcile case data model** -> re-run `demo-builder-data-fabric` in reconciliation mode so stage/task/stub/agent outputs either map to fields or are marked non-persistent.
5. **Build agents.** Dispatch the `agent-builder` sub-agent — **one instance per `AG-*` role, all spawned in a single assistant turn as parallel tool calls** (do not await one before dispatching the next). Enforce 1:1 mapping between `AG-*` IDs and scaffolded agent projects. Never multiplex role prompts in a single runtime.
6. **Build frontend.** Dispatch the `frontend-builder` sub-agent for the UiPath Coded Web App.
7. **Manual completion checklist.** Copy `templates/manual-completion-checklist.template.md` to the build root and fill it with tenant-side work the user must complete.
8. **Produce demo script.** Delegate to `demo-builder-script` — 3-4 key messages, each mapped to 2-3 visuals that actually exist.

See `references/delivery-workflow.md` for the full expanded procedure, completion criteria, and phase I/O.

## Capability contract

| Area | Can do | Cannot do |
|---|---|---|
| Discovery | Web research, summarize operations, extract process patterns | Guarantee domain completeness without SME review |
| Process modeling | Case stage/task diagrams in Mermaid | Directly edit proprietary case editors |
| Automation components | Write RPA/API/IDP specs and contracts | Build inside locked proprietary tooling |
| AI agents | Scaffold and build Python (`uipath-langchain` + `create_agent`) or low-code agents | Bypass tenant/runtime constraints |
| Data Fabric | Produce case entity schema + example JSON for import | Execute tenant-side imports without access |
| Frontend | UiPath Coded Web App with Vite + React + TS SDK | Validate against inaccessible tenant configs |

## Build directory convention

All phase artifacts go in `builds/<demo-slug>/`. Sub-agents read from and write to this directory — never re-derive upstream artifacts. Canonical layout:

```
builds/<demo-slug>/
├── manifest.md          # generated build manifest and validation status
├── manual-completion-checklist.md
├── discovery/           # use-case-research.md, source-register.md, segment-map.md, task-automation-matrix.md
├── case-entity/         # case-entity.schema.json, case-entity.example.json, data-fabric-modeling-notes.md
├── flow-model/          # case-management-flow.mmd, sdd.md, caseplan.json
├── agents/<AG-id>/      # one project per AG-*, plus agent-build-spec.md
├── frontend/            # UiPath Coded Web App
├── handoff/             # RPA/IDP/API component specs
└── script/              # demo-script.md
```

The architect (planner) establishes `<demo-slug>` in Phase 0 and passes the build directory path when dispatching any sub-agent.

## Sub-agent dispatch

When delegating build phases, dispatch to the following sub-agents (defined in `agents/`) instead of invoking sibling skills inline. This isolates context and enables parallelism:

| Phase | Sub-agent | Notes |
|---|---|---|
| 2 — Case data model | `data-modeler` | Initial schema |
| 3 — Case Management design | `case-designer` | Depends on Phase 2 output |
| 4 — Data model reconciliation | `data-modeler` | Re-run after Phase 3 if case/stub/agent outputs need schema changes |
| 5 — Agents | `agent-builder` | **Spawn one instance per `AG-*` in a single assistant turn (multiple Agent tool calls in one message) for true parallel execution.** Sequential dispatch defeats the point — agents are independent. |
| 6 — Frontend | `frontend-builder` | Depends on Phases 2-5 outputs |

Discovery, manual completion checklist, and demo script stay in the main architect thread — invoke `demo-builder-discovery` and `demo-builder-script` skills directly.

Each sub-agent returns a short report; the architect reviews it against the build-directory artifacts before advancing phases.

## Traceability IDs

Keep these stable across artifacts. See `references/mapping-conventions.md` for the full set.

`BR-*` requirements · `SEG-*` segments · `T-*` tasks · `R-*` rules · `G-*` gateways · `CMP-*` components · `AG-*` agents · `MSG-*` demo messages · `VIS-*` demo visuals.

Every `BR-*` must trace to a field and a flow step. Every `SEG-*` contains `T-*`s. Every `AG-*` maps 1:1 to a scaffolded agent project.

## Demo rules

- Code is for demos only. No defensive try/catches, no production hardening. It just needs to work for a small set of pre-defined happy + exception paths with consistent fixture data.
- Prefer mock tools/wrappers when integrations are unavailable. Keep mock/real interfaces identical so replacement is low-friction.
- Pre-stage small varied case sets (normal, urgent, exception).
- See `references/demo-design-principles.md` for UX/story principles.

## References

- `references/delivery-workflow.md` — expanded phase-by-phase procedure
- `references/mapping-conventions.md` — IDs, naming, execution type labels
- `references/demo-design-principles.md` — demo UX and story principles
- `references/research-and-citation-rules.md` — source quality and citation format
- `templates/` — planner-owned artifact templates. Copy templates into `builds/<demo-slug>/` before filling them. Discovery, data-fabric, case-management, agents, frontend, and script templates live in their respective sibling skills.
- `examples/kyc-us-commercial-banking/` — filled example pack

## Sibling skills (delegate to these)

- `demo-builder-discovery` — discovery, segmentation, source register, task matrix templates
- `demo-builder-data-fabric` — case entity schema and example JSON
- `demo-builder-agents` — Python (coded) and low-code agent builds, deploy to Studio Web or Orchestrator
- `demo-builder-case-management` — research→case flow mapping, synthesizes sdd.md, delegates to `uipath-case-management`
- `demo-builder-frontend` — UiPath Coded Web App with Vite + React + UiPath TS SDK demo UI
- `demo-builder-script` — demo script authoring (3-4 messages × 2-3 visuals)

## Success criteria

- Case Management design handed to `uipath-case-management` with a generated `caseplan.json`.
- Manual completion checklist written at `builds/<demo-slug>/manual-completion-checklist.md`.
- Build manifest written at `builds/<demo-slug>/manifest.md` and updated before final handoff.
- Specs written for any proprietary workflows the demo requires (API / RPA / IDP) — omit types the demo doesn't use.
- IDP extraction documents identified (only if the demo uses IDP).
- AI agents fully built and tested with explicit design rationale, tool contracts (real or mock), and 1:1 `uip codedagent` scaffolds. Context Grounding (`index_name` + `folder_path`) and/or MCP URL integration captured when provided by the user.
- Front end matches requirements, runs locally, passes `npm run build`, and has clear `.env` instructions for local testing.
- Suggested demo script with 3-4 key messages × 2-3 visuals each.
