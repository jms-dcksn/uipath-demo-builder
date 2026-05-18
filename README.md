# UiPath Demo Builder Planner

`demo-builder-planner` is a Codex skill for planning demo-grade UiPath Maestro Flow builds. It turns a customer name, use case, or short brief into a precise `SPEC.md`, a short spec-tightening prompt, and a structured Codex `/goal` prompt for long-running demo builds.

The skill plans the demo. It does not build or upload UiPath artifacts by itself. The resulting artifacts are meant to be handed to Codex with the core UiPath skills installed.

## Install

Choose one option.

### Option 1: Ask Codex To Install It

Open Codex and paste:

```text
Install the Codex skill at https://github.com/jms-dcksn/uipath-demo-builder/tree/main/skills/demo-builder-planner
```

Restart Codex after the install finishes.

### Option 2: Install From Terminal

Run:

```bash
python3 ~/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py --repo jms-dcksn/uipath-demo-builder --path skills/demo-builder-planner
```

Restart Codex after the install finishes.

To update later, delete the old installed skill folder and run the same install option again.

## Companion Skills

Install the core UiPath skills too. This planner depends on them for product-specific build guidance:

- `uipath-maestro-flow`
- `uipath-agents`
- `uipath-platform`

## How To Use

Start a Codex conversation and describe the demo you want:

```text
Plan a UiPath Maestro Flow demo for commercial insurance claims triage.
```

Minimum input:

- Customer or account name
- Use case title
- Short use-case brief

Better input:

- Industry or domain
- Known systems or connectors
- Must-show UiPath capabilities
- Happy path and one exception path
- Preferred agent mode, if known
- Studio Web upload expectation, if known

The planner writes:

- `SPEC.md`
- `TIGHTEN-SPEC-PROMPT.md`
- `CODEX-GOAL-PROMPT.md`
- Optional supporting files under `demo-build-plan/` for larger demos

## Workflow

![Demo Builder Planner workflow](docs/planner-workflow.svg)

After the interview, the planner creates a first draft `SPEC.md`. Then ask Codex to tighten the spec by mentioning the files:

```text
@SPEC.md
@TIGHTEN-SPEC-PROMPT.md
```

When the spec is ready, start the implementation handoff:

```text
/goal
@SPEC.md
@CODEX-GOAL-PROMPT.md
```

## Repository Layout

```text
skills/demo-builder-planner/       # Codex skill
skills/demo-builder-planner/SKILL.md
skills/demo-builder-planner/references/spec-quality-checklist.md
skills/demo-builder-planner/references/goal-prompt-template.md
docs/planner-workflow.excalidraw
docs/planner-workflow.svg
```
