# Flow Registry Discovery

## 1) Command Log

| Command | Working Directory | Result Summary | Notes |
|---|---|---|---|
| `uip maestro flow registry search "<name>" --output json` |  |  |  |
| `uip maestro flow registry list --local --output json` |  |  |  |

## 2) Node Type Inventory

| Planned Node | Registry Node Type | Source | Definition Status | Ports Confirmed | Notes |
|---|---|---|---|---|---|
| AG-001 | `uipath.core.agent.<id>` | tenant / local | Pending | Pending |  |
| TOOL-001 | `core.action.script` | local registry | Pending | Pending |  |
| CTRL-001 | `core.logic.decision` | local registry | Pending | Pending |  |

## 3) Resource Discovery

| Resource ID | Name | Type | Discovery Order Used | Found? | Registry Key / Node Type |
|---|---|---|---|---|---|
| AG-001 |  | Agent | tenant then local |  |  |
| CONN-001 |  | Connector Activity | connector registry + connection discovery |  |  |

## 4) Missing Resources

| Resource ID | Missing Item | Recommended Action | Owner |
|---|---|---|---|
| CONN-001 |  | Create/select an Integration Service connection or revise the demo path |  |

## 5) Definition Notes

- Every node type must have a copied registry definition.
- Node instances must not carry `model` blocks.
- Data-producing nodes need instance-level `outputs`.
