---
name: demo-builder-planner
description: "Plan and orchestrate local-execution Maestro Flow demo builds from a use case, industry, or customer brief. Runs preflight -> discovery -> mock API planning -> Flow architecture -> agents -> API workflow build -> Maestro Flow build -> validate/tidy -> Studio Web upload -> manual checklist -> demo script. Use when the user asks to build, design, or scope a UiPath demo."
allowed-tools: Bash, Read, Write, Edit, Glob, Grep, WebFetch, WebSearch, AskUserQuestion, Agent
user-invocable: true
---

# Demo Builder - Planner

Entry point for demo-grade UiPath Maestro Flow builds. The planner owns phase order, user checkpoints, and build-directory coherence. Demos should be simple, working, and useful for presales storytelling.

Default scope: local-execution Flow nodes plus deterministic mock API workflows when they make system-of-record interactions credible. Build around agents as the featured reasoning component, plus mock API workflow resources, connector activities, Flow tool nodes, and Flow control nodes. The default handoff is Studio Web upload so developers can continue editing in the browser. Do not plan live external API calls, RPA process, Human Task, Case Management, Data Fabric, Coded App, frontend, or Orchestrator deployment unless the user explicitly asks to leave this local Flow scope.

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

### Phase 2 - Mock API Planning

Use discovery outputs to decide which `API-*` tasks become deterministic passthrough mock API workflows.

Rules:

- Keep mock API workflows separate from real external API calls.
- Use named systems and operation names, such as `ServiceNowEntitlementLookup`, `GuidewireClaimSummary`, `SAPPurchaseOrderStatus`, `EpicEligibilityCheck`, or `FiservCustomerProfile`.
- Require field alignment before building agents or condition logic. Every field consumed downstream must be present in `discovery/payload-field-map.md`.
- Checkpoint with the user when the system choice is material, such as Epic vs Cerner, Guidewire vs Duck Creek, Salesforce vs ServiceNow, or SAP vs Oracle.
- Record every planned API workflow in `manifest.md`, including whether it will be a real API workflow resource node or a fixture-backed fallback.

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

### Phase 5 - API Workflow Build

Invoke `demo-builder-api-workflows` for each planned `API-*` contract.

Rules:

- Use the passthrough Response-node pattern from `docs/API Workflow-Workflow.json`.
- Write `apis/<API-id>/payload.json`, `apis/<API-id>/api-workflow-contract.md`, and `apis/<API-id>/<WorkflowName>.json`.
- Generate the workflow output schema from the same payload source used for the Response body.
- Validate every mapped downstream JSON path exists in the payload.
- Run `uip api-workflow run <Workflow.json> --no-auth` for each passthrough workflow when the API workflow tool is available.
- Run `uip api-workflow build <projectPath>` and `uip api-workflow pack <projectPath> <destinationPath>` when a project-backed API workflow exists.
- Record validation results, skipped reasons, and registry/solution binding status in `manifest.md`.

### Phase 6 - Flow Build

Invoke `demo-builder-flow` implementation mode.

Hard gates from `uipath-maestro-flow`:

- Create/select a solution before creating the Flow project.
- Use the default node palette plus planned mock API workflow resources only: agents, API workflow nodes, connector activities/triggers, Flow tool nodes, and Flow control nodes.
- Do not add live external API calls, RPA workflow, Human Task, agentic-process, queue, other Flow resource, Coded App, Data Fabric, or Case Management nodes in the default build.
- Discover tenant resources first, then in-solution local resources, then scaffold/create only when needed.
- Validate every node type through registry `search`, `list --local`, and `get`.
- Mock API workflows should use `uipath.core.api-workflow.<resourceKey>` node types when registry discovery returns a bindable resource. Fixture-backed Flow tool injection is only a fallback when the API workflow cannot be created, registered, or bound in time.
- Connector activities require existing Integration Service connections and enriched metadata with `--connection-id`.
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
- Verify expected API workflow projects/resources are present or document the fixture-backed fallback and manual binding step.
- If the uploaded/generated artifact exposes an entry point schema or Studio Web input form, verify the expected manual-start inputs are visible. If this cannot be inspected from CLI output, add a manual checklist item instead of assuming validation proved it.
- If auth is expired, stop and ask the user to re-authenticate or run the relevant `uip login` command for the target tenant.

### Phase 9 - Manual Completion Checklist

Copy `templates/manual-completion-checklist.template.md` to `builds/<demo-slug>/handoff/manual-completion-checklist.md` and fill tenant-side prerequisites:

- Connector connections and folder keys.
- API workflow resources, registry keys, validation status, and fallback status.
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
│   └── <SolutionName>/<FlowProject>/<FlowProject>.flow
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
- `demo-builder-api-workflows` - deterministic passthrough API workflow mock build and validation.
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
- Planned mock API workflows have payloads, contracts, output schemas, run validation, and either bindable API workflow resource nodes or documented fixture fallbacks.
- Required agents are built or discovered and mapped to Flow nodes.
- Connector activities have connection prerequisites resolved or documented as blockers.
- Flow tool/control nodes use registry-backed local node types.
- No live external API call, RPA, Human Task, Case Management, Data Fabric, Coded App, frontend, or other non-local resource build is included unless explicitly requested.
- Manual-start inputs are fixture-backed, bound to the Start/manual trigger, and visible or explicitly marked for Studio Web verification.
- Downstream agent, condition, connector, and End output references to mock API payloads use documented JSON paths.
- `.flow` validates, tidies, and validates again.
- Solution is uploaded to Studio Web with URL and SolutionId captured for developer editing.
- Manual completion checklist captures all tenant-side work.
- Demo script maps to actual Flow nodes, outputs, and visible demo proof.
