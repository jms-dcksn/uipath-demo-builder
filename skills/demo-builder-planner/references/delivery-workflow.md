# Delivery Workflow - Expanded

The planner is Flow-first and local-execution-first. Agents are the primary component; the rest of the Flow should use connector, Flow tool, and Flow control nodes. The default handoff is Studio Web upload for developer browser editing. Deferred orchestration, persistence, and Orchestrator deployment products are not default phases.

## Build Directory

Every phase reads and writes under `builds/<demo-slug>/`. Sub-agents must be given this path and must not re-derive upstream artifacts.

At build start, copy `templates/build-manifest.template.md` to `builds/<demo-slug>/manifest.md`. Append phase status at each boundary.

## Phase 0 - Preflight

Record:

- `uip --version`
- `uip login status --output json`
- `uip maestro flow --help`
- `uip maestro flow registry --help`
- `uip codedagent --help`
- `uip agent --help`
- `uip solution --help`
- `uip is --help`

Abort on missing `uip maestro flow` or `uip solution`.

## Phase 1 - Discovery

Invoke `demo-builder-discovery`.

Completion:

- Source register exists.
- Process map exists.
- Task matrix uses Flow execution types.
- Resource assumptions are explicit.
- Happy path and one exception path are named.
- Non-local resource asks are reframed or recorded as out-of-scope prerequisites.

## Phase 2 - Flow Architecture

Invoke `demo-builder-flow` architecture mode.

Completion:

- `flow/flow-architecture.md`
- `flow/node-contracts.md`
- `flow/registry-discovery.md`
- `flow/connector-bindings.md`

The architect must surface blockers before implementation, especially missing connector connections or missing agent resources.

## Phase 3 - Agents

For each new `AG-*`, dispatch one `agent-builder` instance. Existing agents are documented in `registry-discovery.md` and `node-contracts.md`.

Completion:

- One build spec per new agent.
- Validation/local run status recorded.
- Flow node output contract confirmed.

## Phase 4 - Flow Build

Invoke `demo-builder-flow` implementation mode.

Completion:

- Solution-contained Flow project exists.
- Registry definitions copied for every node type.
- Connector nodes have connection-aware metadata.
- Agent bindings are documented.
- Tool/control nodes stay inside the local Flow execution palette.
- Operator-facing fixture fields are declared as manual-trigger inputs with `direction: "in"` and `triggerNodeId`.
- Agent/tool bindings consume manual-trigger fields through `$vars.<triggerNodeId>.output.<field>` or another documented source.
- Variables and End outputs are mapped.
- Flow write operations were performed sequentially against the `.flow` file.

## Phase 5 - Input Contract, Validate And Tidy

Before validation, inspect the `.flow` file and fixture payloads:

- Every visible fixture field has a matching `variables.globals[]` input.
- Every manual-start input has `triggerNodeId` set to the Start/manual trigger node.
- Downstream bindings use the trigger output path or a documented non-trigger source.
- Missing `triggerNodeId` is a blocker even when `flow validate` succeeds.

Run validate, tidy, validate:

```bash
uip maestro flow validate <FlowProject>.flow --output json
uip maestro flow tidy <FlowProject>.flow --output json
uip maestro flow validate <FlowProject>.flow --output json
```

Completion:

- Latest validation result is recorded.
- Operator input contract check is recorded.
- Tidy result is recorded.
- Any remaining issue is a named blocker.

## Phase 6 - Studio Web Upload

Upload the solution directory to Studio Web:

```bash
uip login status --output json
uip solution upload <SolutionDir> --output json
```

Completion:

- Upload response is recorded in `manifest.md`.
- Studio Web URL is captured and shared.
- `SolutionId` is captured.
- Uploaded project list includes the Flow and expected agent projects.
- Manual-start input visibility is verified from the generated/uploaded schema when available, or marked as a manual Studio Web verification item.
- Any omitted project is a named blocker.

Use `uip solution pack`, `publish`, and `deploy` only when the user explicitly asks for Orchestrator deployment.

## Phase 7 - Manual Completion Checklist

Write `handoff/manual-completion-checklist.md`.

Include:

- Connector connections and folder keys.
- Published/local resource binding status.
- Agent Studio Web upload status or local sibling status.
- Studio Web upload URL, SolutionId, and uploaded projects.
- Manual-start input fields and fixture-to-input mapping.
- Studio Web input-form verification status.
- Optional Orchestrator pack/publish/deploy steps only when requested.
- Debug/run approval items.

## Phase 8 - Demo Script

Invoke `demo-builder-script`.

Completion:

- Script maps to actual Flow proof points.
- Contingency lines exist for auth, connection, or data issues.

## Cross-Phase Completion Criteria

- Every `BR-*` traces to at least one local-execution Flow node or explicit blocker.
- Every `AG-*`, `CONN-*`, `TOOL-*`, and `CTRL-*` is found, built, or documented as missing.
- `.flow` validates after tidy.
- Studio Web upload is complete or blocked with an explicit auth/platform reason.
- Manual checklist captures all tenant-side work.
- Unknowns are marked `TBD` with owner and target date.
