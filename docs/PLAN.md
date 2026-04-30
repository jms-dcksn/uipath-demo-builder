# Demo Builder Local Flow Focus Plan

## Scope

The active plugin should build Maestro Flow demos with local-execution node families only:

- Agents as the primary demo component.
- Connector activities and connector triggers.
- Flow tool nodes for deterministic local work such as scripts and transforms.
- Flow control nodes for trigger, branch, loop, merge, delay, subflow, end, and terminate behavior.

The default build path should not include API workflows, RPA workflows, Human Tasks, Case Management, Data Fabric, Coded Apps, frontends, queues, agentic processes, Orchestrator deployment, or other resource-invocation nodes. If a user explicitly asks for one of those, the planner should pause, name the scope expansion, and route to the relevant UiPath skill.

Every completed build should be uploaded to Studio Web so developers can keep editing there. This is a browser-editing handoff, not an Orchestrator deployment.

## Desired Workflow

`preflight -> discovery -> Flow architecture -> agents -> local Flow build -> validate/tidy -> Studio Web upload -> checklist -> demo script`

## Active Skill Set

- `demo-builder-planner`: owns phase order and keeps the local Flow scope explicit.
- `demo-builder-discovery`: converts a use case into a task matrix with `AI Agent`, `Connector Activity`, `Flow Tool`, `Flow Control`, and `Trigger`.
- `demo-builder-flow`: designs/builds the Maestro Flow and validates registry definitions, connector bindings, outputs, and End mappings.
- `demo-builder-agents`: builds or documents agents used by Flow node contracts.
- `demo-builder-script`: writes the run-of-show from implemented Flow proof points.

## Artifact Shape

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
├── agents/
├── fixtures/
├── handoff/
│   └── manual-completion-checklist.md
└── script/
    └── demo-script.md
```

## Studio Web Handoff

Default command:

```bash
uip solution upload <SolutionDir> --output json
```

Capture the Studio Web URL, `SolutionId`, and uploaded project list in `manifest.md`. Verify the Flow and expected agent projects are present in the upload response. Use `uip solution pack`, `publish`, and `deploy` only when the user explicitly asks for Orchestrator deployment.

## Node ID Conventions

- `AG-*`: agent nodes.
- `CONN-*`: connector activity or connector trigger nodes.
- `TOOL-*`: Flow tool nodes.
- `CTRL-*`: Flow control nodes.
- `FLOW-*`: generic Flow step or End output where useful.
- `PRE-*`: prerequisite or blocker.

## Acceptance Criteria

- README, manifests, commands, skills, templates, and examples describe the local-execution Flow approach.
- No active skill defaults to API workflow, RPA, Human Task, Case Management, Data Fabric, Coded App, frontend, queue, or agentic-process builds.
- Studio Web upload is a required handoff phase after validation/tidy.
- The support-ticket example uses agent, connector, Flow tool, and Flow control nodes only.
- Canonical files and `plugins/demo-builder/` mirror stay in sync.
