# UiPath Demo Builder Skills

Skills for building demo-grade UiPath Maestro Flow solutions from a use case, industry, and requirements. The plugin is optimized for presales demos: simple local-execution flows that feature agents, deterministic mock API workflows, connector activities, Flow tool nodes, and Flow control nodes, then upload the solution to Studio Web so developers can keep editing in the browser.

## What's Included

| Skill | Purpose |
|---|---|
| `demo-builder-planner` | Entry point for Flow-first demo builds. Drives discovery, Flow architecture, agent work, validation, Studio Web upload, checklist, and script. |
| `demo-builder-discovery` | Research, reference-doc intake, process segmentation, and Flow-oriented task matrix. |
| `demo-builder-api-workflows` | Builds project-backed deterministic passthrough API Workflow mocks with hardcoded payloads, matching output schemas, solution registration, and Studio Web upload verification. |
| `demo-builder-flow` | Designs and builds Maestro Flow projects with agents, mock API workflow resources, connector activities, Flow tool nodes, and Flow control nodes. |
| `demo-builder-agents` | Builds or documents UiPath AI agents used by the Flow, either coded or low-code. |
| `demo-builder-script` | Creates the narrated run-of-show from the actual Flow and implemented artifacts. |

## Install

### Claude Code Plugin

In Claude Code, add this repo as a marketplace and install the plugin:

```text
/plugin marketplace add https://github.com/jms-dcksn/uipath-demo-builder.git
/plugin install demo-builder@uipath-demo-builder
```

To update:

```text
/plugin update demo-builder
```

### Codex Plugin Marketplace

This repo includes Codex marketplace metadata at `.agents/plugins/marketplace.json`. Add the repo as a Codex marketplace:

```bash
codex plugin marketplace add https://github.com/jms-dcksn/uipath-demo-builder.git
```

For local testing from this checkout:

```bash
codex plugin marketplace add .
```

When skills change upstream, refresh the Codex marketplace cache and restart Codex App or start a new Codex session:

```bash
codex plugin marketplace upgrade uipath-demo-builder
```

## Companion Skills

Install the UiPath skills from https://github.com/uipath/skills. This plugin relies on them for product-specific behavior:

- `uipath-maestro-flow` for `.flow` structure, registry discovery, node configuration, validation, and tidy.
- `uipath-agents` for coded and low-code agent lifecycle.
- `uipath-platform` for auth, Integration Service connections, Studio Web solution upload, and optional pack/publish/deploy when explicitly requested.
- `uip api-workflow` from `@uipath/api-workflow-tool` for mock API Workflow local run and project build validation.
- `uip solution project add` and `uip solution upload` for API Workflow solution registration and Studio Web confirmation.

## CLI Prerequisites

```bash
npm install -g @uipath/cli   # if `which uip` comes back empty
uip --version
uip maestro flow --help
uip api-workflow --help      # if mock API workflows are in scope
```

If `uip maestro flow` is missing, use `uip tools list`, `uip tools search`, and `uip tools install/update` to install or update the Maestro tool.
If `uip api-workflow` is missing, use `uip tools search api` and `uip tools install @uipath/api-workflow-tool`.

## How To Use

Fastest path:

```text
/demo-builder:demo-build Acme Bank customer onboarding flow
/demo-builder:demo-build path/to/use-case-brief.md
/demo-builder:demo-build
```

The command enters the planner and runs:

`preflight -> discovery -> mock API planning -> Flow architecture -> agents -> project-backed API Workflow build -> Flow invocation binding -> Flow build -> validate/tidy -> Studio Web upload -> checklist -> demo script`

You can also start a conversation in your project directory and describe the demo. The planner activates on requests like "build a UiPath demo", "design a Maestro Flow demo", or "scope a Flow with agents and connectors".

## Minimum Input

- Customer/account name, use case title, or brief.

If only a customer/account is provided, the planner researches and proposes 2-3 Flow demo options before building.

## Ideal Input

- Use case title and one-paragraph business goal.
- Industry and domain.
- Known systems, connectors, tool needs, documents, or agents.
- Demo duration and must-show capabilities.
- Happy path and one exception path.
- Whether agents should be coded, low-code, inline in Flow, or existing tenant resources.

## Example Prompt

```text
Build a Maestro Flow demo for Commercial Insurance Claims Triage.
Business goal: reduce time to first decision while keeping exception handling visible.
Requirements: use a coded claim triage agent, normalize the claim with Flow tool/control nodes, send an adjuster notification through a connector, and return a clear exception result for high-severity claims.
Known systems: Outlook connection, claim intake payload.
Constraints: demo-ready in one week, show happy path and one exception path.
```

## Demo Principles

- Demo-grade, not production. Build the smallest coherent happy path plus one exception path.
- Default to local-execution Flow nodes: deterministic mock API workflows, connectors, Flow tools, Flow control nodes, and agents as the primary featured component.
- Always build mock API workflows as project-backed Studio Web `Api` projects when `API-*` is in scope. Use script placeholders in the Flow only as temporary invocation stand-ins until native inline binding is available.
- Do not plan live external API calls, RPA, Human Task, Case Management, Data Fabric, or frontend builds unless the user explicitly asks to leave the local Flow scope.
- Prefer working Flow nodes over handoff-only specs.
- Use real connector connections when available; stop and document the prerequisite when a connection is missing.
- Prefer connector activities for curated live Integration Service actions when a usable connection exists; otherwise document the connection prerequisite or choose a deterministic mock/API workflow path.
- Discover published resources first, then in-solution siblings, then scaffold only when no suitable resource exists.
- One agent project per `AG-*` role unless the user explicitly chooses an inline Flow agent.
- Validate and tidy every edited Flow before handoff.
- Upload the solution to Studio Web with `uip solution upload <SolutionDir> --output json` and share the returned Studio Web URL for browser editing.
- Do not use `uip solution pack`, `publish`, or `deploy` unless the user explicitly asks for Orchestrator deployment.

## Repository Layout

```text
plugins/demo-builder/ # installable plugin wrapper for marketplace installs
.agents/plugins/      # Codex marketplace manifest
.claude-plugin/       # Claude Code plugin manifest and marketplace
skills/               # canonical skills
agents/               # Claude plugin sub-agents
commands/             # Claude plugin slash commands
builds/               # ignored generated demo builds
docs/                 # ignored local notes except docs/PLAN.md
```

The canonical files and `plugins/demo-builder/` wrapper must stay in sync.

## Reference Docs

- UiPath Flow skill: `uipath-maestro-flow`
- UiPath Python SDK: https://uipath.github.io/uipath-python/
- UiPath LangChain: https://uipath.github.io/uipath-python/langchain/quick_start/
