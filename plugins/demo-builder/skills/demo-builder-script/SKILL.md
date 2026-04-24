---
name: demo-builder-script
description: "Author the narrated demo script (run-of-show) for a UiPath demo. Produces 3-4 key messages each mapped to 2-3 visuals, with narrator line + operator action + visible outcome per beat, opening value statement, closing business impact, and contingency lines. Use when the demo implementation is complete and the user needs a script. Typically invoked last by demo-builder-planner."
allowed-tools: Bash, Read, Write, Edit, Glob, Grep, AskUserQuestion
---

# Demo Builder — Demo Script

Final deliverable: the narrated run-of-show. **Exactly 3-4 key messages**, each mapped to **2-3 implemented visuals**. Script must reflect what was actually built — no future-state narration.

## When to use

- Orchestration, agents, frontend, data, build manifest, and manual completion checklist are in place.
- User asks for a demo script, run-of-show, or demo narrative.

## Narrative pattern (from reference)

1. Open with a business-value statement tied to operational complexity.
2. Show dashboard triage and select a live case/work item.
3. Move into case detail and perform a concrete user action (e.g., upload evidence).
4. Pivot to Maestro orchestration to show adaptive coordination across systems and humans.
5. Drill into one orchestration step to show agent + IDP + integration collaboration.
6. Show a second case state to prove non-linear/adaptive behavior.
7. Return to case detail summary; optional conversational-agent deep dive.
8. Close with decision confidence and business impact.

## Required structure

- **3-4 key messages** (`MSG-*`). Each message ties to a business proof point and one or more `BR-*` requirements.
- **2-3 visuals per message** (`VIS-*`). Pull `VIS-*` IDs tagged in the frontend skill's page templates.
- **Per beat:** narrator line + operator action + observable proof/outcome.
- **Opening:** one value statement.
- **Closing:** one business-impact statement (measurable language, not feature recap).
- **Contingency lines** for predictable live-demo risks (data delay, login failure, etc.).

## Quality checks

- Every message maps to at least one orchestration proof AND one user-facing proof.
- Visual sequence matches implemented screens/flows. No future-state narration.
- Total runtime fits the timebox (default 6-10 minutes unless the user specifies).
- Operator actions were dry-run in order.

## Workflow

1. Copy `templates/demo-script.template.md` to `builds/<demo-slug>/script/demo-script.md` and fill it.
2. Read `builds/<demo-slug>/manifest.md`; only use artifacts marked implemented or explicitly demo-ready.
3. Verify each `VIS-*` exists in the built frontend (cross-reference `demo-builder-frontend` output).
4. Verify each `MSG-*` ties to a `BR-*` and to an observable demo outcome.
5. Add rehearsal checks for any manual-completion checklist item that could affect the live demo.

## Templates

- `templates/demo-script.template.md`

## References

- `references/demo-script-authoring-notes.md` — narrative pattern + quality checks
- `references/demo-design-principles.md` — storytelling principles
