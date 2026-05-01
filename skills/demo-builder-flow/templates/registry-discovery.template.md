# Flow Registry Discovery

## 1) Command Log

| Command | Working Directory | Result Summary | Notes |
|---|---|---|---|
| `uip maestro flow registry search "<name>" --output json` |  |  |  |
| `uip maestro flow registry list --local --output json` |  |  |  |
| `uip api-workflow run <Workflow.json> --no-auth` |  |  | Mock API validation |

## 2) Node Type Inventory

| Planned Node | Registry Node Type | Source | Definition Status | Ports Confirmed | Notes |
|---|---|---|---|---|---|
| API-001 | `uipath.core.api-workflow.<resourceKey>` | in-solution / tenant | Pending | Pending | Category `api-workflow`; binding subtype `Api` |
| AG-001 | `uipath.core.agent.<id>` | tenant / local | Pending | Pending |  |
| TOOL-001 | `core.action.script` | local registry | Pending | Pending |  |
| CTRL-001 | `core.logic.decision` | local registry | Pending | Pending |  |

## 3) Resource Discovery

| Resource ID | Name | Type | Discovery Order Used | Found? | Registry Key / Node Type |
|---|---|---|---|---|---|
| API-001 |  | API Workflow | in-solution then tenant |  |  |
| AG-001 |  | Agent | tenant then local |  |  |
| CONN-001 |  | Connector Activity | connector registry + connection discovery |  |  |

## 4) API Workflow Artifact And Invocation Status

| API ID | API Project Exists | Solution Registered | Studio Web Uploaded | Flow-Bound Node | Invocation Placeholder |
|---|---|---|---|---|---|
| API-001 | Pending | Pending | Pending | Pending native `uipath.core.api-workflow.<resourceKey>` | Script placeholder if native binding is unavailable |

## 5) Missing Resources

| Resource ID | Missing Item | Recommended Action | Owner |
|---|---|---|---|
| API-001 | API workflow registry binding | Keep the API Workflow project registered and use a documented script placeholder until native binding is available |  |
| CONN-001 |  | Create/select an Integration Service connection or revise the demo path |  |

## 6) Definition Notes

- Every node type must have a copied registry definition.
- API workflow definitions should show category `api-workflow`, service type `Orchestrator.ExecuteApiWorkflowAsync`, resource subtype `Api`, and orchestrator type `api`.
- Script-backed API placeholders are invocation fallbacks only; they do not replace built and uploaded API Workflow projects.
- Node instances must not carry `model` blocks.
- Data-producing nodes need instance-level `outputs`.
