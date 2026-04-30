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
| Studio Web input form verified | Expected manual-start fields visible | Pending |  | If CLI cannot verify, inspect Studio Web before demo |

## 2) Agents

| Agent ID | Source | Project Path / Registry Name | Local Run | Validation | Studio Web Upload Status | Notes |
|---|---|---|---|---|---|---|
| AG-001 | coded / low-code / inline / existing |  | Pending | Pending | Pending |  |

## 3) Connectors

| Connector ID | Connector Key | Connection ID | Folder Key | State | Manual Action |
|---|---|---|---|---|---|
| CONN-001 |  |  |  | Pending | Create/select connection if missing |

## 4) Flow Tools And Controls

| Node ID | Node Family | Registry Node Type | Status | Manual Action |
|---|---|---|---|---|
| TOOL-001 | Flow Tool |  | Pending | None unless registry definition is unavailable |
| CTRL-001 | Flow Control |  | Pending | None unless registry definition is unavailable |

## 5) Out-Of-Scope Requests

| Request | Why It Is Out Of Default Scope | Owner Decision |
|---|---|---|
|  | API workflow / RPA / Human Task / Case Management / Data Fabric / frontend |  |

## 6) Demo Readiness

| Item | Status | Notes |
|---|---|---|
| Happy path fixture maps to manual-start inputs | Pending |  |
| Exception path fixture maps to manual-start inputs | Pending |  |
| Happy path fixture tested | Pending |  |
| Exception path fixture tested | Pending |  |
| Flow debug/run approved by user | Pending | Do not run debug without explicit consent |
| Demo script dry run completed | Pending |  |
| Fallback narration prepared | Pending |  |

## 7) Optional Orchestrator Deployment

Only fill this section when the user explicitly asks to deploy beyond Studio Web.

| Item | Status | Notes |
|---|---|---|
| Solution packed | Not requested | `uip solution pack` |
| Solution published | Not requested | `uip solution publish` |
| Solution deployed | Not requested | `uip solution deploy run` |
