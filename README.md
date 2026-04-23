# UiPath Demo Builder Skills

A set of Claude Code skills for building **demo-grade** UiPath solutions from a use case, industry, and requirements. Optimized for visual storytelling, not production hardening.

## What's included

| Skill | Purpose |
|---|---|
| `demo-builder-planner` | Orchestrator. Entry point for any demo build. Drives the end-to-end workflow and delegates to sibling skills. |
| `demo-builder-discovery` | Web research, segmentation, task-automation matrix, BPMN vs Case Management recommendation. |
| `demo-builder-data-fabric` | Case entity schema + example records for UiPath Data Fabric import. |
| `demo-builder-case-management` | Demo-grade Case Management design. Produces minimal `sdd.md` and delegates to the production `uipath-case-management` skill. |
| `demo-builder-agents` | Demo-grade AI agents (coded via `uipath-langchain` + `create_agent`, or low-code `agent.json`). 1:1 agent-project scaffolding. |
| `demo-builder-frontend` | Vite + React + TypeScript app using the UiPath TypeScript SDK. Multi-page shell, dashboard, case detail, UI-to-entity data contract. |
| `demo-builder-script` | Narrated run-of-show. 3-4 key messages × 2-3 visuals each. |

## Install

### Recommended: as a Claude Code plugin

In Claude Code, add this repo as a marketplace and install the plugin:

```
/plugin marketplace add <this-repo-url>
/plugin install demo-builder@uipath-demo-builder
```

That's it — all 7 skills and 4 sub-agents are registered. To update: `/plugin update demo-builder`.

```bash
git clone <this-repo-url> ~/src/demo-builder
mkdir -p ~/.claude/skills ~/.claude/agents
ln -s ~/src/demo-builder/skills/demo-builder-planner          ~/.claude/skills/demo-builder-planner
ln -s ~/src/demo-builder/skills/demo-builder-discovery        ~/.claude/skills/demo-builder-discovery
ln -s ~/src/demo-builder/skills/demo-builder-data-fabric      ~/.claude/skills/demo-builder-data-fabric
ln -s ~/src/demo-builder/skills/demo-builder-case-management  ~/.claude/skills/demo-builder-case-management
ln -s ~/src/demo-builder/skills/demo-builder-agents           ~/.claude/skills/demo-builder-agents
ln -s ~/src/demo-builder/skills/demo-builder-frontend         ~/.claude/skills/demo-builder-frontend
ln -s ~/src/demo-builder/skills/demo-builder-script           ~/.claude/skills/demo-builder-script
for a in agent-builder case-designer data-modeler frontend-builder; do
  ln -s ~/src/demo-builder/agents/$a.md ~/.claude/agents/$a.md
done
```

### Required companion skills

These skills delegate deep work to the production UiPath skills. Install them from the UiPath Skills repo: https://github.com/uipath/skills

- `uipath-agents` — deep agent lifecycle, framework selection, evals, HITL, `agent.json` format
- `uipath-case-management` — `sdd.md` → `tasks.md` → `caseplan.json` generation via `uip maestro case`
- `uipath-data-fabric` — production Data Fabric CRUD, CSV import, attachments
- `uipath-platform` — Orchestrator/Studio Web publish

### CLI prerequisite

```bash
npm install -g @uipath/cli   # if `which uip` comes back empty
```

## How to use

Fastest path — use the slash command:

```
/demo-build Acme Bank KYC
/demo-build path/to/use-case-brief.md
/demo-build            # no args — it will ask for a customer or brief
```

The command deterministically enters the planner skill and runs discovery → data model → case management → agents → frontend → demo script.

Or start a conversation in your project directory and describe the demo. The planner skill auto-activates on phrases like "build a UiPath demo", "design a demo for…", or "scope a UiPath use case".

### Minimum input
- Customer/account name → planner will research the account and propose 2-3 use-case options.

### Ideal input
- Use case title + one-paragraph business goal
- Industry and domain
- Requirements and constraints
- Known systems/APIs/documents

### Example prompt

```text
Build a new demo for "Commercial Insurance Claims Triage".
Business goal: reduce time to first decision by 40% while improving consistency of risk assessment.
Industry: Insurance / Claims Operations.
Requirements: Use UiPath Maestro, Data Fabric, at least 2 Python agents, React frontend with dashboard + case detail.
Known systems: Policy admin API, document inbox, adjuster notes in SharePoint.
Constraints: demo-ready in 2 weeks, show happy path + one exception path, include a final demo script.
```

The planner walks the 10-phase delivery workflow, delegating each phase to the appropriate sibling skill.

## Delivery workflow (high level)

1. Gather inputs (or research the account)
2. Research the operation → sources + process patterns
3. Decompose into 3-4 segments
4. Task automation matrix (`AI Agent` / `RPA` / `IDP` / `API` / `Human Task`)
5. Choose orchestration (BPMN vs Case Management)
6. Case data model (Data Fabric entity schema + examples)
7. Orchestration design (Maestro BPMN or Case Management)
8. Build AI agents (one scaffold per `AG-*`)
9. Build frontend (dashboard + case detail)
10. Handoff specs for proprietary components (RPA/IDP/API) + demo script (3-4 messages × 2-3 visuals)

See `skills/demo-builder-planner/references/delivery-workflow.md` for the full procedure.

## Demo principles

- **Demo-grade, not production.** No defensive programming, no excessive try/catch. Happy path + one exception path.
- **Consistent over live.** Pre-staged data and mock tools (with real-tool-shaped interfaces) beat half-working integrations.
- **Traceability.** Every `BR-*` → `T-*` → entity field → UI element → `MSG-*` → `VIS-*`.
- **One agent project per `AG-*` role.** No prompt multiplexing.

## Repository layout

```
skills/              # the skills themselves (installable)
context/             # original template library (legacy, pre-skills)
builds/              # example filled builds
docs/                # design notes and updates
```

`context/` is retained as a reference library. The skills are the installable artifact.

## Reference docs

- UiPath LangChain: https://uipath.github.io/uipath-python/langchain/quick_start/
- UiPath TypeScript SDK: https://uipath.github.io/uipath-typescript/getting-started/
