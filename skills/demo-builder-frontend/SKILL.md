---
name: demo-builder-frontend
description: "Build the Vite + React + TypeScript frontend for a UiPath demo using the UiPath TypeScript SDK. Defines multi-page app with full-width header, collapsible sidebar, dashboard worklist, case detail page, and an explicit UI-to-entity data contract. Use when the demo needs a custom front end. Typically invoked by demo-builder-planner after case entity and orchestration are defined."
allowed-tools: Bash, Read, Write, Edit, Glob, Grep, AskUserQuestion
---

# Demo Builder — Frontend

Vite + React + TypeScript app using the UiPath TypeScript SDK. **Demo-grade** — optimize for visual appeal and story-telling, not production hardening.

## When to use

- Demo needs a custom front end (dashboard + case detail at minimum).
- UI must showcase orchestration state, agent outputs, and case progression.

## Inputs

- Case entity schema (from `demo-builder-data-fabric`)
- Task automation matrix (from `demo-builder-discovery`)
- Orchestration design (BPMN or Case Management)

## Reference

- UiPath TypeScript SDK getting started: `https://uipath.github.io/uipath-typescript/getting-started/`

## Workflow

1. Fill `templates/frontend-architecture.template.md` — app structure, data flow, UiPath TS SDK integration model, routing, state strategy.
2. Fill `templates/dashboard-page.template.md` — multi-page shell with full-width header, collapsible sidebar (with use-case-relevant menu placeholders), and case/process worklist. Clicking a row routes to detail.
3. Fill `templates/case-detail-page.template.md` — instance detail route for case/process context, variables, metadata, and decision actions (user task actions map to backend operations).
4. Fill `templates/ui-data-contract.template.md` — exact mapping from UI elements to entity fields AND task-level data. Every displayed field needs a source and fallback behavior.
5. **Env setup:** give the user clear `.env` instructions for local testing (tenant URL, auth, identifiers).
6. **Placeholder methods:** if Data Fabric case entities aren't created yet, document placeholder methods that swap to real SDK calls after entity creation.

## Demo rules

- Pre-stage a small varied set of cases (normal / urgent / exception).
- Make state transitions visible after operator actions.
- Dashboard answers "What needs attention now?"; detail page answers "What decision should I make now?"
- Tag visuals with `VIS-*` IDs so the demo script can reference them.
- No defensive programming. Happy path + one exception path is enough.

## Completion criteria

- Multi-page app with header + collapsible sidebar.
- Dashboard worklist + case detail route implemented.
- Every UI field maps to an entity field or task field with a source + fallback.
- User task actions wired to backend operations.
- `.env` instructions provided for local run.
- Screens tagged with `VIS-*` IDs usable by the demo script.

## Templates

- `templates/frontend-architecture.template.md`
- `templates/dashboard-page.template.md`
- `templates/case-detail-page.template.md`
- `templates/ui-data-contract.template.md`
