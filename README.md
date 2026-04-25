# UiPath Demo Builder Skills

A set of Claude Code skills for building **demo-grade** UiPath solutions from a use case, industry, and requirements. Optimized for visual storytelling, not production hardening.

## What's included

| Skill | Purpose |
|---|---|
| `demo-builder-planner` | Orchestrator. Entry point for any demo build. Drives the end-to-end workflow and delegates to sibling skills. |
| `demo-builder-discovery` | Web research, segmentation, task-automation matrix, and Case Management-ready process boundaries. |
| `demo-builder-data-fabric` | Case entity schema + example records for UiPath Data Fabric import. |
| `demo-builder-case-management` | Demo-grade Case Management design. Produces minimal `sdd.md` for architect-owned `uipath-case-management` generation. |
| `demo-builder-agents` | Demo-grade AI agents (coded via `uipath-langchain` + `create_agent`, or low-code `agent.json`). 1:1 agent-project scaffolding. |
| `demo-builder-frontend` | UiPath Coded Web App using Vite + React + TypeScript and the UiPath TypeScript SDK. Multi-page shell, dashboard, case detail, UI-to-entity data contract. |
| `demo-builder-script` | Narrated run-of-show. 3-4 key messages × 2-3 visuals each. |

## Install

### Recommended: as a Claude Code plugin

In Claude Code, add this repo as a marketplace and install the plugin:

```
/plugin marketplace add https://github.com/jms-dcksn/uipath-demo-builder.git
/plugin install demo-builder@uipath-demo-builder
```

That's it — all 7 skills and 4 sub-agents are registered. To update: `/plugin update demo-builder`.

### Also available as a Codex plugin marketplace

This repo also includes Codex marketplace metadata at `.agents/plugins/marketplace.json`. Add the repo as a Codex marketplace:

```bash
codex plugin marketplace add https://github.com/jms-dcksn/uipath-demo-builder.git
```

Then install or enable `demo-builder` from the Codex plugin marketplace UI. For local testing from this checkout, use:

```bash
codex plugin marketplace add .
```

### Required companion skills

These skills delegate deep work to the production UiPath skills. Install them from the UiPath Skills repo: https://github.com/uipath/skills

- `uipath-agents` — deep agent lifecycle, framework selection, evals, HITL, `agent.json` format
- `uipath-case-management` — `sdd.md` → `tasks.md` → `caseplan.json` generation via `uip maestro case`
- `uipath-data-fabric` — production Data Fabric CRUD, CSV import, attachments
- `uipath-coded-apps` — coded web/action app scaffold, build, package, publish, deploy
- `uipath-platform` — Orchestrator/Studio Web publish

### CLI prerequisites

```bash
npm install -g @uipath/cli   # if `which uip` comes back empty
uip tools install @uipath/codedapp-tool
```

## How to use

Fastest path — use the slash command:

```
/demo-builder:demo-build Acme Bank KYC
/demo-builder:demo-build path/to/use-case-brief.md
/demo-builder:demo-build            # no args — it will ask for a customer or brief
```

The command deterministically enters the planner skill and runs preflight -> discovery -> data model -> Case Management design -> caseplan generation -> agents -> fixture check -> frontend -> schema reconcile -> manual completion checklist -> demo script.

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

The planner walks the Case-Management-only delivery workflow, delegating each phase to the appropriate sibling skill.

## Delivery workflow (high level)

1. Run CLI preflight (`uip`, Case Management, coded agent, and solution surfaces)
2. Gather inputs (or research the account)
3. Research the operation → sources + process patterns
4. Decompose into 3-4 segments
5. Task automation matrix (`AI Agent` / `RPA` / `IDP` / `API` / `Human Task`)
6. Confirm Case Management stage boundaries
7. Case data model (Data Fabric entity schema + examples)
8. Case Management design (`case-management-design.md`, `sdd.md`)
9. Architect-owned `uipath-case-management` generation (`tasks.md`, `caseplan.json`)
10. Build AI agents (one scaffold per `AG-*`)
11. Fixture consistency check against deterministic agent outputs
12. Build UiPath Coded Web App frontend (dashboard + case detail)
13. Frontend/schema reconciliation
14. Manual completion checklist + demo script (3-4 messages × 2-3 visuals)

See `skills/demo-builder-planner/references/delivery-workflow.md` for the full procedure.

## Demo principles

- **Demo-grade, not production.** No defensive programming, no excessive try/catch. Happy path + one exception path.
- **Consistent over live.** Pre-staged data and mock tools (with real-tool-shaped interfaces) beat half-working integrations.
- **Traceability.** Every `BR-*` → `T-*` → entity field → UI element → `MSG-*` → `VIS-*`.
- **One agent project per `AG-*` role.** No prompt multiplexing.

## Repository layout

```
plugins/demo-builder/ # dual-format plugin wrapper for marketplace installs
.agents/plugins/      # Codex marketplace manifest
.claude-plugin/       # Claude Code plugin manifest and marketplace
skills/              # the skills themselves (installable)
agents/              # Claude plugin sub-agents
commands/            # Claude plugin slash commands
builds/              # ignored generated demo builds
docs/                # ignored local working notes
```

The skills, agents, and commands are the installable artifact. Generated builds and local docs are ignored by git.

## Reference docs

- UiPath LangChain: https://uipath.github.io/uipath-python/langchain/quick_start/
- UiPath TypeScript SDK: https://uipath.github.io/uipath-typescript/getting-started/
