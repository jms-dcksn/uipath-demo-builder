# Mock API Workflow Contract

## 1) Identity

- API ID: `API-001`
- Workflow name:
- Named system:
- Operation name:
- Business purpose:
- Mock status: deterministic passthrough
- Runnable JSON path: `apis/API-001/<WorkflowName>.json`
- API project directory: `flow/<SolutionName>/<ApiProjectName>/`
- `project.uiproj` path: `flow/<SolutionName>/<ApiProjectName>/project.uiproj`
- Workflow `document.tags.projectId`:
- Workflow `document.tags.solutionId`:
- Solution manifest project ID:
- Entry point unique ID:
- Upload project name:
- Upload `projectType`: `Api`

## 2) Source Notes

| Source ID | Source Type | Used For | Notes |
|---|---|---|---|
| SRC-001 | Public API docs / connector docs / sample payload / user doc | Payload convention |  |

## 3) Response Payload Summary

| JSON Path | Example Value | Field Class | Downstream Consumer | Demo Visible? |
|---|---|---|---|---|
| `$.account.accountId` | `ACC-100245` | Identity | AG-001 | Yes |
| `$.entitlement.status` | `active` | Decision | CTRL-001 | Yes |
| `$.entitlement.summary` | `Premium support active` | Narrative | AG-001 | Yes |

## 4) Output Schema

- Schema source: generated from `payload.json`.
- Response node source: same `payload.json` content.
- Drift check:

## 5) Project-Backed API Workflow

| Artifact | Required Value | Status | Notes |
|---|---|---|---|
| `Workflow.json` | Passthrough Response-node workflow | Pending | Same output schema and payload source as `payload.json` |
| `project.uiproj` | `ProjectType: Api`, `MainFile: Workflow.json` | Pending |  |
| `entry-points.json` | `type: Api`, output schema from payload | Pending |  |
| `bindings_v2.json` | `version: 2.0`, resources array | Pending | Empty resources for pure passthrough mocks |
| `.local/ProjectSettings.json` | Empty breakpoint/watch/activity metadata | Pending |  |
| Solution manifest entry | `Type: Api`, project relative path | Pending | Required before upload |
| Upload response | `projectType: Api` for this API project | Pending | Required before final handoff |

## 6) Flow Invocation

| Flow Field | Value |
|---|---|
| Planned Flow node ID |  |
| Native registry node type | `uipath.core.api-workflow.<resourceKey>` |
| Node category | `api-workflow` |
| Model service type | `Orchestrator.ExecuteApiWorkflowAsync` |
| Binding resource subtype | `Api` |
| Binding orchestrator type | `api` |
| Output variable when natively bound | `$vars.<apiNodeId>.output` |
| Current invocation mode | `bound API node` / `script placeholder` / `not used` |
| Script placeholder node |  |
| Placeholder output contract | Must match `payload.json` and API output schema |
| Native binding blocker |  |
| Future replacement note | Replace script placeholder with native API Workflow node when inline binding is available |

## 7) Validation

| Check | Command or Review | Result | Notes |
|---|---|---|---|
| Payload JSON valid | Structured parser | Pending |  |
| Field map paths exist | JSON path review | Pending |  |
| Workflow JSON valid | Structured parser | Pending |  |
| Local API workflow run | `uip api-workflow run <Workflow.json> --no-auth` | Pending |  |
| Project scaffold exists | File review | Pending | `Workflow.json`, `project.uiproj`, `entry-points.json`, `bindings_v2.json`, `.local/ProjectSettings.json` |
| Project build | `uip api-workflow build <projectPath> --output json` | Pending | Required |
| Solution registration | `uip solution project add <projectPath> <SolutionFile> --output json` plus `.uipx` review | Pending | `.uipx` must contain `Type: Api` |
| Upload confirmation | `uip solution upload <SolutionDir> --output json` response review | Pending | Response must contain this project with `projectType: Api` |

## 8) Validation State

| State | Status | Evidence |
|---|---|---|
| JSON valid | Pending |  |
| Local run passed | Pending |  |
| Project build passed | Pending |  |
| Solution registered | Pending |  |
| Upload confirmed | Pending |  |

## 9) Safety Review

- No real customer data:
- No real patient data:
- No real account numbers:
- No credentials or live endpoints:
