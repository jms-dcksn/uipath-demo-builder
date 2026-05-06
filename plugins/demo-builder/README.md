# UiPath Demo Builder Codex Plugin

Codex plugin marketplace package for planning and scaffolding demo-grade UiPath Agentic Orchestration solutions from a customer, account, or use-case brief.

## Codex Install

From the repository root, add the local marketplace:

```bash
codex plugin marketplace add .
```

If the packaged Codex CLI cannot run because of Windows execution policy or app package restrictions, add this repository through the Codex plugin marketplace UI from the desktop app.

## What Loads In Codex

- `.codex-plugin/plugin.json` is the plugin manifest.
- `skills/` contains the Codex-loadable demo-builder skills.
- `agents/` contains Codex subagent profile documents used by the planner when the host exposes subagent delegation.
- `commands/demo-build.md` is a pasteable Codex workflow prompt, not an executable command surface.

## Start A Demo Build

Ask Codex to use the planner skill, for example:

```text
Use the demo-builder-planner skill to build a UiPath demo for Commercial Insurance Claims Triage.
```

The planner runs preflight, discovery, data modeling, Case Management design, agent planning, frontend planning, manual checklist generation, and demo script authoring under `builds/<demo-slug>/`.

## UiPath CLI Preflight

Before a real build, verify these commands locally:

```bash
uip --version
uip login status --output json
uip maestro case --help
uip codedagent --help
uip solution --help
```

If a required `uip` surface is missing, stop at preflight and update the local UiPath CLI/tooling before producing downstream artifacts.
