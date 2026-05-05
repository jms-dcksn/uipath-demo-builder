---
name: demo-builder-planner
description: "Plan and orchestrate local-execution Maestro Flow demo builds from a use case, industry, or customer brief. Runs preflight -> discovery -> mock API planning -> Flow architecture -> agents -> project-backed API Workflow build -> Flow invocation binding -> Maestro Flow build -> validate/tidy -> Studio Web upload -> manual checklist -> demo script. Use when the user asks to build, design, or scope a UiPath demo."
allowed-tools: Bash, Read, Write, Edit, Glob, Grep, WebFetch, WebSearch, AskUserQuestion, Agent
user-invocable: true
---

# Demo Builder - Planner

Entry point for demo-grade UiPath Maestro Flow builds. The planner owns phase order, user checkpoints, and build-directory coherence. Demos should be simple, working, and useful for presales storytelling.

Default scope: local-execution Flow nodes plus deterministic mock API workflows when they make system-of-record interactions credible. Build around agents as the featured reasoning component, plus project-backed mock API Workflow projects, connector activities, Flow tool nodes, and Flow control nodes. The default handoff is Studio Web upload so developers can continue editing in the browser. Do not plan live external API calls, RPA process, Human Task, Case Management, Data Fabric, Coded App, frontend, or Orchestrator deployment unless the user explicitly asks to leave this local Flow scope.

## When To Use

- User asks to build, design, scope, or propose a UiPath demo.
- User mentions Maestro Flow, agents, connector activities, Flow tools, Flow control nodes, or agentic orchestration.
- User provides a customer/account name and wants demo ideas.
- User provides a use-case brief.

## Not For

- Production automation design.
- Non-local resource orchestration outside the local Flow scope.
- Running existing automations unless the user explicitly asks to debug or run them.

## Inputs

Ideal:

- Use case title and one-paragraph business goal.
- Industry/domain.
- Known systems, connectors, tool needs, agents, documents, and tenant/folder constraints.
- Demo duration.
- Happy path and one exception path.

Minimum:

- Customer/account name. Research and propose 2-3 Flow demo options before continuing.

## Delivery Workflow

Every phase writes under `builds/<demo-slug>/`. Copy `templates/build-manifest.template.md` to `builds/<demo-slug>/manifest.md` at build start and append phase status at every boundary.

### Phase 0 - Preflight

Run and record:

- `uip --version`
- `uip login status --output json`
- `uip maestro flow --help`
- `uip maestro flow registry --help`
- `uip codedagent --help`
- `uip agent --help`
- `uip solution --help`
- `uip is --help`
- `uip api-workflow --help` when mock API workflows are in scope, or record that `uip tools install @uipath/api-workflow-tool` is needed.

If `uip maestro flow` or `uip solution` is absent, stop and surface the blocker. Flow projects must be created inside a solution.

### Phase 1 - Discovery

Invoke `demo-builder-discovery` for research, reference-doc intake, process segmentation, and the task automation matrix.

The matrix must identify Flow-buildable work using only these execution types:

- `AI Agent`
- `Mock API Workflow`
- `Connector Activity`
- `Flow Tool`
- `Flow Control`
- `Trigger`

Checkpoint with the user before architecture when the resource mix, named system selection, connector assumptions, or API mock assumptions are unclear. If discovery reveals live external APIs, RPA, Human Task, Case Management, Data Fabric, Coded App, or frontend needs, either reframe them as deterministic mock API workflow, connector/tool/control/agent behavior or record them as out-of-scope prerequisites.

For every planned `CONN-*`, discovery must capture the connector service, activity intent, required connection or folder, expected user-visible output, and whether the connector is a live action or only a prerequisite.

### Phase 2 - Mock API Planning

Use discovery outputs to decide which `API-*` tasks become deterministic passthrough mock API workflows.

Rules:

- Keep mock API workflows separate from real external API calls.
- Use named systems and operation names, such as `ServiceNowEntitlementLookup`, `GuidewireClaimSummary`, `SAPPurchaseOrderStatus`, `EpicEligibilityCheck`, or `FiservCustomerProfile`.
- Require field alignment before building agents or condition logic. Every field consumed downstream must be present in `discovery/payload-field-map.md`.
- Checkpoint with the user when the system choice is material, such as Epic vs Cerner, Guidewire vs Duck Creek, Salesforce vs ServiceNow, or SAP vs Oracle.
- Record every planned API workflow in `manifest.md` as both an artifact and an invocation decision. The artifact is always a project-backed Studio Web API Workflow; the Flow invocation can be a native API Workflow node when available or a script-backed placeholder with the same output contract while native inline binding is unavailable.

### Phase 3 - Flow Architecture

Invoke `demo-builder-flow` to produce:

- `flow/flow-architecture.md`
- `flow/node-contracts.md`
- `flow/registry-discovery.md`
- `flow/connector-bindings.md`

The architecture must name required API workflow resources, agents, connector activities, Flow tool/control node types, registry definitions, Flow variables, End outputs, connector/API bindings, payload JSON paths, and open prerequisites.

### Phase 4 - Agents

If new agents are required, dispatch one `agent-builder` instance per `AG-*` role. Existing published or in-solution agents do not require a build; document their binding in `flow/node-contracts.md`.

Rules:

- One `AG-*` per standalone agent project unless the user explicitly selects inline Flow agent.
- Prefer existing local sibling agents or new inline/standalone agents for demo-contained builds. Use published tenant agents when the user names one.
- Coded path defaults to `uipath-langchain` and `create_agent`.
- Low-code path uses `uip agent init`.
- Inline agents use `uip agent init <FlowProjectDir> --inline-in-flow`.
- Output contracts are Flow node contracts, not persisted entity fields.
- Agent inputs sourced from mock API payloads must reference documented JSON paths, such as `$vars.<apiNodeId>.output.coverageStatus`.

### Phase 5 - API Workflow Artifact Build

Invoke `demo-builder-api-workflows` for each planned `API-*` contract.

Rules:

- Use the passthrough Response-node pattern from `docs/API Workflow-Workflow.json`.
- Create or select the solution directory before writing API Workflow project folders if it does not already exist.
- Write `apis/<API-id>/payload.json`, `apis/<API-id>/api-workflow-contract.md`, and `apis/<API-id>/<WorkflowName>.json`.
- Generate the workflow output schema from the same payload source used for the Response body.
- Validate every mapped downstream JSON path exists in the payload.
- Run `uip api-workflow run <Workflow.json> --no-auth` for each passthrough workflow when the API workflow tool is available.
- Create one API Workflow project folder under the solution for every planned `API-*`.
- Each API project folder must include `Workflow.json`, `project.uiproj`, `entry-points.json`, `bindings_v2.json`, and `.local/ProjectSettings.json`.
- Run `uip api-workflow build <projectPath> --output json` for every project-backed API Workflow.
- Run `uip solution project add <projectPath> <SolutionFile> --output json` for every API Workflow project.
- Inspect the `.uipx` and confirm one `Type: "Api"` project entry per planned `API-*`.
- Record validation results, skipped reasons, project paths, solution manifest IDs, and Flow invocation readiness in `manifest.md`.

### Phase 5b - Flow Invocation Binding Decision

After project-backed API Workflows exist, decide how the Flow invokes each `API-*` contract.

Rules:

- Prefer a native `uipath.core.api-workflow.<resourceKey>` Flow node when registry discovery exposes a reliable bindable resource.
- If native binding is not available yet, use a script-backed placeholder node that returns the same payload shape as the API Workflow output schema.
- Script-backed placeholders are temporary invocation shims only. They do not satisfy API Workflow artifact build, solution registration, or upload verification.
- Record each `API-*` invocation status as `bound API node`, `script placeholder`, or `not used`.

### Phase 6 - Flow Build

Invoke `demo-builder-flow` implementation mode.

Hard gates from `uipath-maestro-flow`:

- Create/select a solution before creating the Flow project.
- Use the default node palette plus planned mock API workflow resources only: agents, API workflow nodes, connector activities/triggers, Flow tool nodes, and Flow control nodes.
- Do not add live external API calls, RPA workflow, Human Task, agentic-process, queue, other Flow resource, Coded App, Data Fabric, or Case Management nodes in the default build.
- Discover tenant resources first, then in-solution local resources, then scaffold/create only when needed.
- Validate every node type through registry `search`, `list --local`, and `get`.
- Mock API workflows should use `uipath.core.api-workflow.<resourceKey>` node types when registry discovery returns a bindable resource. Until native binding is available, script-backed Flow placeholders may invoke equivalent mock payloads, but the API Workflow projects must still be built and registered.
- Connector activities require existing Integration Service connections and enriched metadata with `--connection-id`.
- Connector activities can block Flow implementation when no enabled connection exists or required/reference fields cannot be resolved.
- Every node type needs a copied registry definition.
- Every external demo fixture field that the operator must provide is declared as a manual-trigger input with `direction: "in"` and `triggerNodeId` pointing to the Start/manual trigger node.
- Agent/tool bindings consume manual-trigger fields through the trigger output path, such as `$vars.start.output.<field>`, or document a different source.
- Every data-producing node needs an `outputs` block.
- Every edge needs `targetPort`.
- Use `=js:` for `$vars`, `$metadata`, and `$self` references in value fields.
- Map every `out` variable on every reachable End node.
- Do not run `flow debug` without explicit user consent.
- Flow file mutations must be sequential. Do not run `uip maestro flow node add/configure/delete`, direct JSON writes, or `tidy` in parallel against the same `.flow` file.

### Phase 7 - Input Contract, Validate And Tidy

Before validation, perform a static operator input contract check:

- Compare `fixtures/happy-path.json` and `fixtures/exception-path.json` keys against the operator input surface in `flow/node-contracts.md`.
- Inspect `.flow` and confirm each visible manual-start input appears in `variables.globals[]` with `direction: "in"` and `triggerNodeId`.
- Confirm downstream bindings use `$vars.<triggerNodeId>.output.<field>` or another documented source.
- Confirm every downstream agent input, condition branch, connector input, and End output sourced from an API workflow points to a documented payload JSON path.
- Confirm every planned `CONN-*` has a selected or blocked connection, folder key, enriched registry metadata, required field values, reference field resolution status, and connector parameter mappings recorded in `flow/connector-bindings.md`.
- Confirm every planned `API-*` has a project build result, a solution `.uipx` `Type: "Api"` entry, and a Flow invocation status of `bound API node`, `script placeholder`, or `not used`.
- Record this check in `manifest.md`. Treat missing `triggerNodeId` as a handoff blocker, even if `flow validate` passes.

Run:

```bash
uip maestro flow validate <FlowProject>.flow --output json
uip maestro flow tidy <FlowProject>.flow --output json
uip maestro flow validate <FlowProject>.flow --output json
```

Record validation and tidy results in `manifest.md`.

### Phase 8 - Studio Web Upload

Upload the solution for browser editing:

```bash
uip login status --output json
uip solution upload <SolutionDir> --output json
```

Rules:

- `<SolutionDir>` is the folder containing the `.uipx` file.
- Use `uip solution upload`, not `uip solution pack`, `publish`, or `deploy`.
- Parse and record the returned Studio Web URL, `SolutionId`, and uploaded projects in `manifest.md`.
- Inspect the upload response and verify the Flow project and expected agent projects are present. If a local agent is omitted, mark the upload incomplete and surface the blocker.
- Compare expected projects against actual uploaded projects by name and `projectType`.
- Verify one uploaded `projectType: "Api"` project per planned `API-*`. If any planned API Workflow is missing, mark upload incomplete.
- Record Flow invocation status separately from upload status. A script placeholder is acceptable only as the current Flow invocation mode, not as a replacement for uploaded API Workflow projects.
- Record connector readiness separately from upload status. A successful Studio Web upload does not prove that a live connector activity has a valid connection, reference IDs, or required field values.
- If the uploaded/generated artifact exposes an entry point schema or Studio Web input form, verify the expected manual-start inputs are visible. If this cannot be inspected from CLI output, add a manual checklist item instead of assuming validation proved it.
- If auth is expired, stop and ask the user to re-authenticate or run the relevant `uip login` command for the target tenant.

### Phase 9 - Manual Completion Checklist

Copy `templates/manual-completion-checklist.template.md` to `builds/<demo-slug>/handoff/manual-completion-checklist.md` and fill tenant-side prerequisites:

- Connector connections and folder keys.
- Connector keys, activity node types, operation metadata, required fields, reference lookups, parameter mappings, and unresolved prerequisites.
- API workflow project paths, solution manifest IDs, upload `projectType` values, registry keys, validation status, and Flow invocation status.
- Published or local resource bindings.
- Agent Studio Web upload status or local sibling status.
- Studio Web URL, SolutionId, and uploaded project list.
- Manual-start input fields, fixture-to-input mapping, and Studio Web input-form verification status.
- Any optional Orchestrator pack/publish/deploy steps requested by the user.
- Manual debug/run approval items.

### Phase 10 - Demo Script

Invoke `demo-builder-script`. Script only against implemented or explicitly demo-ready Flow artifacts.

## Build Directory Convention

```text
builds/<demo-slug>/
├── manifest.md
├── discovery/
├── flow/
│   ├── flow-architecture.md
│   ├── node-contracts.md
│   ├── registry-discovery.md
│   ├── connector-bindings.md
│   └── <SolutionName>/
│       ├── <FlowProject>/<FlowProject>.flow
│       └── <ApiProjectName>/
│           ├── Workflow.json
│           ├── project.uiproj
│           ├── entry-points.json
│           ├── bindings_v2.json
│           └── .local/ProjectSettings.json
├── apis/
│   └── API-001/
│       ├── api-workflow-contract.md
│       ├── payload.json
│       └── <WorkflowName>.json
├── agents/
├── fixtures/
│   ├── happy-path.json
│   └── exception-path.json
├── handoff/
│   └── manual-completion-checklist.md
└── script/
    └── demo-script.md
```

## Sibling Skills

- `demo-builder-discovery` - research, segmentation, and Flow-oriented task matrix.
- `demo-builder-api-workflows` - project-backed deterministic passthrough API Workflow mock build, solution registration, and validation.
- `demo-builder-flow` - Flow architecture, registry discovery, connector binding plan, implementation, validate/tidy.
- `demo-builder-agents` - coded/low-code/inline agent build guidance.
- `demo-builder-script` - final run-of-show.

Companion UiPath skills:

- `uipath-maestro-flow`
- `uipath-agents`
- `uipath-platform`

## Success Criteria

- Flow project lives inside a solution.
- Flow architecture and node contracts are written.
- Planned mock API workflows have payloads, contracts, output schemas, run validation, project-backed API Workflow folders, successful project builds, `.uipx` `Type: "Api"` registration, and upload confirmation as `projectType: "Api"`.
- Flow invocation for each planned `API-*` is explicitly marked as native API node, script placeholder, or not used.
- Required agents are built or discovered and mapped to Flow nodes.
- Connector activities have connection prerequisites, required fields, and reference fields resolved or documented as blockers.
- Flow tool/control nodes use registry-backed local node types.
- No live external API call, RPA, Human Task, Case Management, Data Fabric, Coded App, frontend, or other non-local resource build is included unless explicitly requested.
- Manual-start inputs are fixture-backed, bound to the Start/manual trigger, and visible or explicitly marked for Studio Web verification.
- Downstream agent, condition, connector, and End output references to mock API payloads use documented JSON paths.
- `.flow` validates, tidies, and validates again.
- Solution is uploaded to Studio Web with URL and SolutionId captured for developer editing.
- Manual completion checklist captures all tenant-side work.
- Demo script maps to actual Flow nodes, outputs, and visible demo proof.
