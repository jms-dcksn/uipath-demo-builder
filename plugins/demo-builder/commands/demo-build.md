# Demo Build Workflow Prompt

Use this prompt text when you want Codex to start a UiPath demo-builder workflow from this plugin.

## Input

Provide a customer, account, use case, or path to a use-case brief. Examples:

- `Acme Bank KYC`
- `Commercial Insurance Claims Triage`
- `docs/brief.md`

## Codex Procedure

1. Load the `demo-builder-planner` skill. If Codex does not auto-load it, read `skills/demo-builder-planner/SKILL.md` and follow it literally.
2. Treat the user's prompt as the initial input:
   - If it looks like a file path, read it and use it as the use-case brief.
   - If it names a customer or account only, run the minimum-input branch: research the account, propose 2-3 use cases, ask the user to select one, then continue.
   - If it describes a use case, proceed straight to discovery.
   - If it is empty, ask the user for a customer name or use-case brief before doing anything else.
3. Run the UiPath CLI preflight before creating downstream artifacts:
   - `uip --version`
   - `uip login status --output json`
   - `uip maestro case --help`
   - `uip codedagent --help`
   - `uip solution --help`
4. Follow the planner delivery phases without skipping. Write all generated artifacts under `builds/<demo-slug>/` per the planner's build directory convention.
5. Use Codex subagent profiles (`agent-builder`, `frontend-builder`, `case-designer`, `data-modeler`) when the host exposes subagent delegation. Fan out `agent-builder` once per `AG-*` role in one parallel turn when available. If subagent delegation is unavailable, keep the same artifact boundaries and state the limitation in the manifest.

## Pasteable Start

```text
Use the demo-builder-planner skill for this input: <customer, account, use case, or brief path>
```