# Manual Completion Checklist

Tenant-side and operator steps that cannot be completed from local artifacts.

## 1) Flow Solution

| Item | Required Value | Status | Owner | Notes |
|---|---|---|---|---|
| Solution created | solution name/path | Pending |  | Flow project must be inside solution |
| Flow validates | validation result | Pending |  | `uip maestro flow validate` |
| Flow tidied | tidy result | Pending |  | `uip maestro flow tidy` |
| Operator input contract documented | `flow/node-contracts.md` input surface | Pending |  | Fixture fields map to manual-start inputs |
| Manual-start inputs bound to trigger | `.flow` `variables.globals[].triggerNodeId` | Pending |  | Required for Studio Web manual input form |
| Solution uploaded to Studio Web | Studio Web URL + SolutionId | Pending |  | `uip solution upload <SolutionDir> --output json` |
| Uploaded projects verified | Flow + agent project list | Pending |  | Check upload response before handoff |
| API workflow projects verified | API workflow project/resource list | Pending |  | Confirm mock API workflows are uploaded as `projectType: Api` |
| API workflow invocation status documented | `flow/registry-discovery.md` | Pending |  | Native API node, script placeholder, or not used |
| Studio Web input form verified | Expected manual-start fields visible | Pending |  | If CLI cannot verify, inspect Studio Web before demo |

## 2) Agents

| Agent ID | Source | Project Path / Registry Name | Context Grounding | Additional Tool | Local Run | Validation | Studio Web Upload Status | Notes |
|---|---|---|---|---|---|---|---|---|
| AG-001 | coded / low-code / inline / existing |  | none / existing index / new index required | GenAI Activity web search / connector / MCP / mock / none | Pending | Pending | Pending |  |

## 2a) Context Grounding And Agent Tools

| Agent ID | Index Name | Folder Path | Source Documents / Topics | Manual Index Creation Status | Additional Tool Readiness | Demo Fallback |
|---|---|---|---|---|---|---|
| AG-001 |  |  |  | Not needed / pending / complete / blocked | Ready / manual setup / mocked / blocked | Deterministic mock/stub or not needed |

## 3) Connectors

| Connector ID | Node Type | Connector Key | Activity | Connection ID | Folder Key | Ping / Health | Manual Action |
|---|---|---|---|---|---|---|---|
| CONN-001 |  |  |  |  |  | Pending | Create/select connection if missing |

| Connector ID | Operation | Method | Endpoint | Required Fields | Reference Fields | Parameter Mapping Status |
|---|---|---|---|---|---|---|
| CONN-001 |  |  |  | Pending | Pending | Pending |

## 4) Mock API Workflows

| API ID | Named System | API Project Path | Local Run | Project Build | Solution Registered | Upload `projectType` | Flow Invocation |
|---|---|---|---|---|---|---|---|
| API-001 |  | `flow/<Solution>/<ApiProjectName>/` | Pending | Pending | Pending `Type: Api` | Pending `Api` | `bound API node` / `script placeholder` / `not used` |

## 5) Flow Tools And Controls

| Node ID | Node Family | Registry Node Type | Status | Manual Action |
|---|---|---|---|---|
| TOOL-001 | Flow Tool |  | Pending | None unless registry definition is unavailable |
| CTRL-001 | Flow Control |  | Pending | None unless registry definition is unavailable |

## 6) Out-Of-Scope Requests

| Request | Why It Is Out Of Default Scope | Owner Decision |
|---|---|---|
|  | Live API call / RPA / Human Task / Case Management / Data Fabric / frontend |  |

## 7) Demo Readiness

| Item | Status | Notes |
|---|---|---|
| Happy path fixture maps to manual-start inputs | Pending |  |
| Exception path fixture maps to manual-start inputs | Pending |  |
| API payload fields map to downstream consumers | Pending |  |
| Mock API workflow local run completed or skipped with reason | Pending |  |
| API Workflow project build completed | Pending |  |
| API Workflow uploaded as `projectType: Api` | Pending |  |
| Script placeholders clearly labeled as temporary invocation stand-ins | Pending |  |
| Connector connection and field readiness documented | Pending | A successful Studio Web upload does not prove live connector runtime readiness |
| Happy path fixture tested | Pending |  |
| Exception path fixture tested | Pending |  |
| Flow debug/run approved by user | Pending | Do not run debug without explicit consent |
| Demo script dry run completed | Pending |  |
| Fallback narration prepared | Pending |  |

## 8) Optional Orchestrator Deployment

Only fill this section when the user explicitly asks to deploy beyond Studio Web.

| Item | Status | Notes |
|---|---|---|
| Solution packed | Not requested | `uip solution pack` |
| Solution published | Not requested | `uip solution publish` |
| Solution deployed | Not requested | `uip solution deploy run` |
