---
name: demo-builder-flow
description: "Design and build demo-grade UiPath Maestro Flow projects using local-execution node types: AI agents, connector activities/triggers, Flow tool nodes, Flow control nodes, and End outputs. Uses the installed uipath-maestro-flow skill for node-specific authoring rules."
allowed-tools: Bash, Read, Write, Edit, Glob, Grep, AskUserQuestion
---

# Demo Builder - Maestro Flow

Owns Flow architecture and implementation for demo-builder. Defer node-specific `.flow` rules to `uipath-maestro-flow`; this skill coordinates the demo artifacts and build gates.

Default scope: agents are the primary component. The rest of the Flow should use only local-execution node families: connectors, Flow tools, and Flow control nodes. The finished solution should be uploadable to Studio Web for developer editing. Do not introduce API workflow, RPA workflow, Human Task, Case Management, Data Fabric, Coded App, frontend, queue, agentic-process, Orchestrator deployment, or other resource-invocation nodes unless the user explicitly asks to leave this scope.

## When To Use

- Planner has discovery output and needs a Flow topology.
- User wants a Maestro Flow demo with agents, connector activities, Flow tool nodes, or Flow control nodes.
- Existing Flow needs demo-oriented extension.

## Inputs

- Use-case brief and research.
- Process map.
- Task automation matrix.
- Known tenant folder, connector names, connection names, tool requirements, and agent names.
- Agent build specs or existing agent resource names.
- Happy path and exception fixture payloads.
- Fixture field list and the fields the demo operator must enter when starting the Flow manually.

## Flow Architecture Workflow

1. Copy templates into `builds/<demo-slug>/flow/`:
   - `templates/flow-architecture.template.md`
   - `templates/node-contracts.template.md`
   - `templates/registry-discovery.template.md`
   - `templates/connector-bindings.template.md`
2. Map each task to a Flow node:
   - `AG-*` for agent nodes.
   - `CONN-*` for Integration Service connector activities.
   - `TOOL-*` for Flow tool nodes such as script and transform.
   - `CTRL-*` for Flow control nodes such as trigger, decision, switch, loop, merge, delay, end, terminate, and subflow.
3. Define the operator input surface before internal wiring:
   - Identify every external field from the happy path and exception fixtures.
   - Decide which fields are visible manual-start inputs and which are supplied by defaults, upstream nodes, or fixture-backed tools.
   - For manual-trigger demos, each visible input must map to a `variables.globals[]` item with `direction: "in"` and `triggerNodeId` set to the trigger node ID, normally `start`.
   - Downstream nodes should reference visible trigger inputs through the trigger output path, such as `$vars.start.output.documentExcerpt`, not a bare `$vars.documentExcerpt`.
4. Define remaining variables, End outputs, branch conditions, and exception path.
5. Run resource discovery:
   - Tenant registry search for named published agents.
   - In-solution local registry for sibling agents.
   - Connector registry search for connector activities/triggers.
   - Registry `get` for every Flow tool/control node type.
   - Scaffold agents only when no suitable resource exists and the user wants it created.
6. For connectors, confirm connections before implementation.
7. Write open prerequisites instead of guessing connection IDs, required fields, reference IDs, or folder keys.

## Implementation Workflow

Use the installed `uipath-maestro-flow` skill for exact node recipes.

1. Create or select a solution.
2. Create the Flow project inside the solution.
3. Discover registry definitions for every node type.
4. Add the manual-trigger input contract before agent/tool wiring.
5. Add/configure nodes.
6. Add workflow variables and End output mappings directly in the `.flow` file.
7. Wire edges with explicit `targetPort`.
8. Add `outputs` blocks to every data-producing node.
9. Use `=js:` for `$vars`, `$metadata`, and `$self` references in value fields.
10. Validate the operator input surface with a static `.flow` review:
    - Every visible fixture field has a matching `direction: "in"` global variable.
    - Every visible manual input has `triggerNodeId` pointing to the Start/manual trigger node.
    - Every downstream binding uses the trigger output path or another documented source.
11. Validate/tidy/validate.
12. Hand the solution directory to the planner for Studio Web upload with `uip solution upload <SolutionDir> --output json`.

## Manual Trigger Input Rules

- Treat the demo operator input surface as a first-class contract, not an implementation detail.
- `variables.globals[]` with `direction: "in"` is not enough by itself for manual-start demos. Add `triggerNodeId` so Studio Web can associate the input with the Start/manual trigger entry point.
- Keep fixture keys, trigger input IDs, agent input variables, Flow tool inputs, and End output examples coherent.
- `uip maestro flow validate` is required, but it does not prove Studio Web exposes the manual-start input form correctly. Record the input-surface check separately in the manifest and handoff checklist.
- If the current CLI or uploaded artifact provides an `entry-points.json` or equivalent generated schema, inspect it after build/upload and verify the expected input properties are present.

## Connector Rules

- Use connector activity nodes when a curated activity exists.
- Use connector triggers when the demo should start from a supported external event.
- Use connector-backed managed HTTP only when the user approves it as a connector workaround.
- Do not use manual HTTP as a default local-execution demo node. If no connector exists, document the gap or ask the user whether to leave the local-only scope.
- Before building a connector node:
  - `uip is connections list <connector-key> --output json`
  - choose a default enabled connection or ask the user
  - `uip maestro flow registry get <nodeType> --connection-id <connection-id> --output json`
  - resolve required fields and reference fields
- If no connection exists, stop and document the prerequisite in `flow/connector-bindings.md`.

## Flow Tool Rules

- Prefer tool nodes for deterministic local work: payload normalization, shaping agent inputs, parsing agent output, static transforms, and fixture-backed enrichment.
- Use `core.action.script` for small JavaScript transforms and parsing.
- Use `core.action.transform*` only when the transform is static enough for the native transform node.
- Do not call external services from script nodes.
- Keep tool outputs small and shaped for downstream agents, connectors, control branches, or End output.

## Flow Control Rules

- Use control nodes for topology and runtime pathing: manual/scheduled trigger, decision, switch, loop, merge, delay, subflow, end, and terminate.
- Model the exception path with control nodes and End outputs by default. Do not add Human Task/HITL nodes unless the user explicitly asks for Action Center.
- Every reachable path must end in a mapped End node or terminate node.

## Flow Mutation Rules

- Registry reads, file reads, and planning checks may run in parallel.
- Do not run Flow file mutations in parallel against the same `.flow` file. Run `uip maestro flow node add`, `node configure`, `node delete`, direct JSON edits, `tidy`, and other write operations sequentially.
- After any CLI mutation, re-read the changed `.flow` before applying direct JSON edits so the next write is based on current state.

## Agent Rules

- Published agents use tenant registry discovery.
- Sibling standalone agents use `uip maestro flow registry list --local --output json`.
- Inline agents use `uip agent init <FlowProjectDir> --inline-in-flow --output json` and `uipath.agent.autonomous`.
- The words coded and low-code describe agent implementation style, not inline status.
- For inline agents, inspect the current registry output or CLI-generated node shape before final JSON edits. Prefer current generated fields over older examples when `inputs.source`, prompts, input variables, or output variables drift.
- Agent outputs are consumed from `$vars.<nodeId>.output.content` unless the node definition says otherwise.

## Out-Of-Scope Resource Rules

- API workflows, RPA workflows, Human Tasks, queues, other Flow resources, and agentic processes are not part of the default local-execution build.
- When the user's use case mentions one of these resources, first try to satisfy the demo with an agent, connector, Flow tool, or Flow control node.
- If the external resource is truly required, record it as an explicit prerequisite or ask the user to expand scope before wiring it.

## Completion Criteria

- `flow-architecture.md`, `node-contracts.md`, `registry-discovery.md`, and `connector-bindings.md` are complete.
- All required agents/connectors are found, scaffolded, or documented as blockers.
- All tool/control nodes are local registry-backed node types.
- Flow project is solution-contained.
- Manual-trigger demo inputs are documented, fixture-backed, bound with `triggerNodeId`, and referenced through the trigger output path or another documented source.
- `.flow` validates, tidies, and validates again.
- Solution directory is ready for `uip solution upload <SolutionDir> --output json`.
- Manual checklist includes tenant-side connections, Studio Web upload, manual-start input surface verification, optional Orchestrator deployment, or debug/run steps.

## Templates

- `templates/flow-architecture.template.md`
- `templates/node-contracts.template.md`
- `templates/registry-discovery.template.md`
- `templates/connector-bindings.template.md`
