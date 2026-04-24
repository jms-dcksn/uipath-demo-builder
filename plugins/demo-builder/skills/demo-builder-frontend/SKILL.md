---
name: demo-builder-frontend
description: "Build a UiPath Coded Web App frontend for a UiPath demo using Vite + React + TypeScript and the UiPath TypeScript SDK. Defines multi-page app with full-width header, collapsible sidebar, dashboard worklist, case detail page, explicit UI-to-entity data contract, Coded Web App config, OAuth/env setup, local run, and production build. Typically invoked by demo-builder-planner after case entity and Case Management design are defined."
allowed-tools: Bash, Read, Write, Edit, Glob, Grep, AskUserQuestion
---

# Demo Builder — Frontend

UiPath Coded Web App using Vite + React + TypeScript and the UiPath TypeScript SDK. **Demo-grade** — optimize for visual appeal and story-telling, not production hardening.

## When to use

- Demo needs a custom UiPath Coded Web App front end (dashboard + case detail at minimum).
- UI must showcase orchestration state, agent outputs, and case progression.

## Inputs

- Case entity schema (from `demo-builder-data-fabric`)
- Task automation matrix (from `demo-builder-discovery`)
- Case Management design
- Agent output contracts
- Stub contracts and manual completion checklist

## Reference

- UiPath TypeScript SDK getting started: `https://uipath.github.io/uipath-typescript/getting-started/`
- Installed `uipath-coded-apps` skill for Coded Web App scaffold, auth, build, pack, publish, and deploy.

## Workflow

1. Copy `templates/frontend-architecture.template.md` to `builds/<demo-slug>/frontend/frontend-architecture.md` and fill it with app structure, data flow, UiPath TS SDK integration model, routing, state strategy, and Coded Web App deployment assumptions.
2. Copy `templates/dashboard-page.template.md` to `builds/<demo-slug>/frontend/dashboard-page.md` and fill it. The shell must include a full-width header, collapsible sidebar, use-case-relevant menu placeholders, and case/process worklist. Clicking a row routes to detail.
3. Copy `templates/case-detail-page.template.md` to `builds/<demo-slug>/frontend/case-detail-page.md` and fill it. The instance detail route shows case context, variables, metadata, agent/stub outputs, and decision actions.
4. Copy `templates/ui-data-contract.template.md` to `builds/<demo-slug>/frontend/ui-data-contract.md` and fill it. Every displayed field maps to an entity field, task field, agent output, or documented fallback.
5. Build the actual Coded Web App under `builds/<demo-slug>/frontend/<app-name>/`. Use the installed `uipath-coded-apps` skill for deep details.
6. Configure Vite for UiPath hosting:
   - `vite.config.ts` must set `base: './'`.
   - Client-side routing must use `getAppBase()` from `@uipath/uipath-typescript`.
7. **Env setup:** provide `.env.example` with `VITE_UIPATH_CLIENT_ID`, `VITE_UIPATH_SCOPE`, `VITE_UIPATH_ORG_NAME`, `VITE_UIPATH_TENANT_NAME`, `VITE_UIPATH_BASE_URL`, and any app-specific IDs. Do not commit real secrets.
8. **Placeholder methods:** if Data Fabric case entities aren't created yet, document placeholder methods that swap to real SDK calls after entity creation.
9. Run `npm install`, `npm run dev` for local boot, and `npm run build` for production validation.
10. Record pack/publish/deploy commands and any External Application/OAuth setup in `manual-completion-checklist.md`.

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
- `vite.config.ts` uses `base: './'`.
- Router basename uses `getAppBase()`.
- `npm run build` succeeds or failure is documented.
- Screens tagged with `VIS-*` IDs usable by the demo script.

## Templates

- `templates/frontend-architecture.template.md`
- `templates/dashboard-page.template.md`
- `templates/case-detail-page.template.md`
- `templates/ui-data-contract.template.md`
