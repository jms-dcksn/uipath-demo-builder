# Build Manifest

## 1) Build Metadata

- Demo slug:
- Customer/account:
- Use case:
- Flow solution:
- Flow project:
- Build owner:
- Last updated:

## 2) Phase Status

| Phase | Owner Skill/Agent | Status | Primary Artifact(s) | Validation |
|---|---|---|---|---|
| Preflight | demo-builder-planner | PENDING | CLI version/login status | Pending |
| Discovery | demo-builder-discovery | PENDING | `discovery/use-case-research.md`, `discovery/source-register.md`, `discovery/process-map.md`, `discovery/task-automation-matrix.md` | Pending |
| Mock API planning | demo-builder-planner | PENDING | `discovery/system-interactions.md`, `discovery/mock-api-payload-research.md`, `discovery/payload-field-map.md` | Pending |
| Flow architecture | demo-builder-flow | PENDING | `flow/flow-architecture.md`, `flow/node-contracts.md`, `flow/registry-discovery.md`, `flow/connector-bindings.md` | Pending |
| Connector readiness | demo-builder-flow | PENDING | `flow/connector-bindings.md`, `flow/node-contracts.md` | Pending |
| Agents | agent-builder / demo-builder-agents | PENDING | `agents/<AG-id>/` + per-agent tool plan | Pending |
| API workflow artifact build | demo-builder-api-workflows | PENDING | `apis/<API-id>/`, `flow/<Solution>/<ApiProjectName>/` | Pending |
| API workflow invocation binding | demo-builder-flow | PENDING | `flow/registry-discovery.md`, `flow/node-contracts.md` | Pending |
| Flow build | demo-builder-flow | PENDING | `flow/<Solution>/<Project>/<Project>.flow` | Pending |
| Input contract check | demo-builder-flow | PENDING | trigger input mapping + `.flow` review | Pending |
| Validate and tidy | demo-builder-flow | PENDING | validation/tidy output | Pending |
| Studio Web upload | demo-builder-planner | PENDING | Studio Web URL, SolutionId, upload response | Pending |
| Manual checklist | demo-builder-planner | PENDING | `handoff/manual-completion-checklist.md` | Pending |
| Demo script | demo-builder-script | PENDING | `script/demo-script.md` | Pending |

## 3) Artifact Register

| Artifact | Path | Produced By | Depends On | Status |
|---|---|---|---|---|
| Source register | `discovery/source-register.md` | demo-builder-discovery | User docs + web sources | PENDING |
| System interactions | `discovery/system-interactions.md` | demo-builder-discovery | Use-case research | PENDING |
| Payload research | `discovery/mock-api-payload-research.md` | demo-builder-discovery | User docs + web/API sources | PENDING |
| Payload field map | `discovery/payload-field-map.md` | demo-builder-discovery | Payload research | PENDING |
| Flow architecture | `flow/flow-architecture.md` | demo-builder-flow | Task matrix | PENDING |
| Node contracts | `flow/node-contracts.md` | demo-builder-flow | Flow architecture | PENDING |
| Connector bindings | `flow/connector-bindings.md` | demo-builder-flow | Integration Service discovery | PENDING |
| Connector node contracts | `flow/node-contracts.md` | demo-builder-flow | Connector bindings | PENDING |
| Agent tool readiness | `discovery/task-automation-matrix.md`, `agents/<AG-id>/agent-build-spec.md`, `flow/node-contracts.md` | demo-builder-discovery / demo-builder-agents | User preferences + tenant prerequisites | PENDING |
| API workflow contract | `apis/API-001/api-workflow-contract.md` | demo-builder-api-workflows | Payload field map | PENDING |
| API workflow payload | `apis/API-001/payload.json` | demo-builder-api-workflows | Payload research | PENDING |
| API workflow JSON | `apis/API-001/<WorkflowName>.json` | demo-builder-api-workflows | Payload JSON | PENDING |
| API workflow project | `flow/<Solution>/<ApiProjectName>/project.uiproj` | demo-builder-api-workflows | Workflow JSON + output schema | PENDING |
| API workflow entry point | `flow/<Solution>/<ApiProjectName>/entry-points.json` | demo-builder-api-workflows | Payload output schema | PENDING |
| Flow project | `flow/<Solution>/<Project>/<Project>.flow` | demo-builder-flow | Registry discovery | PENDING |
| Agent project | `agents/<AG-id>/` | agent-builder | Agent build spec | PENDING |
| Studio Web upload response | `manifest.md` | demo-builder-planner | Validate and tidy | PENDING |
| Operator input mapping | `flow/node-contracts.md` | demo-builder-flow | Fixtures + Flow architecture | PENDING |

## 4) Validation Register

| Check | Command or Review | Result | Notes |
|---|---|---|---|
| Flow validate before tidy | `uip maestro flow validate <file> --output json` | Pending |  |
| Manual trigger input contract | Static `.flow` review | Pending | Confirm `direction: "in"` globals include `triggerNodeId` and fixture fields are covered |
| Flow tidy | `uip maestro flow tidy <file> --output json` | Pending |  |
| Flow validate after tidy | `uip maestro flow validate <file> --output json` | Pending |  |
| Agent validation/local run | agent-specific command | Pending | One row per agent in notes |
| Agent Context Grounding and tool readiness | `task-automation-matrix.md` + `agent-build-spec.md` review | Pending | Confirm index/folder details, manual setup status, and additional tool readiness |
| API payload JSON | Structured parser | Pending | One row per `API-*` |
| API field map paths | Static JSON path review | Pending | Confirm every downstream path exists in payload |
| API workflow local run | `uip api-workflow run <Workflow.json> --no-auth` | Pending | One row per mock API workflow |
| API workflow project scaffold | File review | Pending | Confirm `Workflow.json`, `project.uiproj`, `entry-points.json`, `bindings_v2.json`, `.local/ProjectSettings.json` |
| API workflow project build | `uip api-workflow build <projectPath> --output json` | Pending | Required for every planned `API-*` |
| API workflow solution registration | `.uipx` review | Pending | Confirm one `Type: Api` entry per planned `API-*` |
| API workflow upload confirmation | Upload response review | Pending | Confirm one `projectType: Api` per planned `API-*` |
| API workflow invocation mode | `flow/registry-discovery.md` review | Pending | `bound API node`, `script placeholder`, or `not used` |
| Connector connection health | `uip is connections list --folder-key/ping` | Pending | One row per connector; record selected connection and folder key |
| Connector enriched metadata | `uip maestro flow registry get <nodeType> --connection-id <connection-id> --output json` | Pending | One row per connector activity |
| Connector required/reference fields | `flow/connector-bindings.md` review | Pending | Confirm required values and reference IDs are resolved or blocked |
| Connector parameter expressions | `flow/node-contracts.md` + `.flow` review | Pending | Confirm `$vars`, `$metadata`, and `$self` references use `=js:` |
| Tool/control node registry check | `uip maestro flow registry get <nodeType> --output json` | Pending | One row per local node family in notes |
| Studio Web upload | `uip solution upload <SolutionDir> --output json` | Pending | Record Studio Web URL, SolutionId, and uploaded projects |
| Studio Web input form | Generated schema or manual Studio Web inspection | Pending | Confirm expected manual-start fields are visible |
| Demo script dry run | Manual rehearsal | Pending |  |

## 5) Studio Web Handoff

- Studio Web URL:
- SolutionId:
- Uploaded project names:
- Uploaded project IDs:
- API workflow resources:
- API workflow registry keys:
- API workflow project paths:
- API workflow solution manifest IDs:
- API workflow upload `projectType` values:
- API workflow invocation status:
- Agent Context Grounding indexes:
- Agent Context Grounding folder paths:
- Agent source-document prerequisites:
- Agent additional tool readiness:
- Connector keys:
- Connector activity node types:
- Connector connection IDs:
- Connector folder keys:
- Connector operations:
- Connector method/endpoint values:
- Connector unresolved prerequisites:
- Visible manual-start input fields:
- Fixture-to-input mapping:
- Input form verification:
- Upload command:
- Upload status:
- Omitted expected projects:

## 6) Open Issues

| ID | Issue | Owner | Target Resolution | Status |
|---|---|---|---|---|
| OI-001 |  |  |  | Open |
