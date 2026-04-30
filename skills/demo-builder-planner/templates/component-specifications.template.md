# Local Flow Node Specifications Template

Use this to capture local Flow nodes and explicit out-of-scope asks. Default builds should not depend on API workflows, RPA workflows, Human Tasks, Case Management, Data Fabric, Coded Apps, frontends, queues, or other resource-invocation nodes.

## 1) Node Index

| Node ID | Node Family | Task IDs | Owner | Build Status |
|---|---|---|---|---|
| TOOL-001 | Flow Tool | T-003 |  | Planned |

## 2) Node Spec

Copy this section once per node.

### Node: `<Node ID>`

- Type: `Agent | Connector Activity | Flow Tool | Flow Control | Trigger`
- Business purpose:
- Flow node that invokes it:
- Inputs:
- Outputs:
- Upstream dependencies:
- Downstream dependencies:
- Exception cases:
- Auth/connection requirements:
- Demo fallback:

## 3) Flow Tool And Control Notes

| Node ID | Registry Node Type | Local Logic | Input Shape | Output Shape |
|---|---|---|---|---|
| TOOL-001 | `core.action.script` |  |  |  |

## 4) Connector Notes

| Connector ID | Connector Key | Activity | Required Connection | Required Fields |
|---|---|---|---|---|
| CONN-001 |  |  |  |  |

## 5) Out-Of-Scope Requests

| Request | Excluded Node/Product | Reason | Owner Decision |
|---|---|---|---|
|  | API workflow / RPA / Human Task / Case Management / Data Fabric / frontend | Outside local-execution Flow scope |  |
