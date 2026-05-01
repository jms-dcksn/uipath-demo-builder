---
name: demo-builder-script
description: "Author the narrated run-of-show for a local-execution Maestro Flow demo. Produces 3-4 key messages mapped to actual Flow nodes, runtime outcomes, mock API workflow, agent/connector/tool/control proof points, and visible operator actions."
allowed-tools: Bash, Read, Write, Edit, Glob, Grep, AskUserQuestion
---

# Demo Builder - Demo Script

Final deliverable: a concise narrated run-of-show. Script only what exists or is explicitly demo-ready.

## When To Use

- Flow architecture, Flow build/validation status, agent status, and manual checklist exist.
- User asks for a demo script, run-of-show, or narrative.

## Required Structure

- 3-4 key messages (`MSG-*`).
- 2-3 proof points per message (`VIS-*` or `FLOW-*`).
- Per beat: narrator line, operator action, visible outcome.
- Opening value statement.
- Closing business-impact statement.
- Contingency lines for predictable demo risks.

## Workflow

1. Copy `templates/demo-script.template.md` to `builds/<demo-slug>/script/demo-script.md`.
2. Read `manifest.md`, `flow/flow-architecture.md`, `flow/node-contracts.md`, and `handoff/manual-completion-checklist.md`.
3. Use only implemented or explicitly demo-ready artifacts.
4. Map messages to actual proof points: trigger, uploaded API Workflow project, native API Workflow node, script-backed API placeholder, agent result, connector activity, Flow tool output, control branch, End output, or Studio Web Flow visualization.
5. If API Workflow projects exist but the Flow uses script placeholders, say both facts plainly:
   - API Workflows are present as separate Studio Web `Api` projects.
   - The current Flow uses equivalent script-backed placeholders for local execution until native inline binding is available.
6. Do not imply a Flow node is an API Workflow node unless the uploaded Flow actually contains that node type.
7. Add rehearsal checks for any manual checklist item that could affect the live demo.

## Quality Checks

- No future-state narration.
- Every key message maps to an observable Flow outcome.
- API Workflow proof points distinguish project presence from Flow invocation mode.
- Runtime fits the timebox.
- Operator actions were dry-run or marked as pending.

## Templates

- `templates/demo-script.template.md`
