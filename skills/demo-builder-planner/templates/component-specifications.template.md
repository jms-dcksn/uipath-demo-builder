# Local Flow Node Specifications Template

Use this to capture local Flow nodes, deterministic mock API workflows, and explicit out-of-scope asks. Default builds should not depend on live external API calls, RPA workflows, Human Tasks, Case Management, Data Fabric, Coded Apps, frontends, queues, or other resource-invocation nodes.

## 1) Node Index

| Node ID | Node Family | Task IDs | Owner | Build Status |
|---|---|---|---|---|
| API-001 | Mock API Workflow | T-002 |  | Planned |
| TOOL-001 | Flow Tool | T-003 |  | Planned |

## 2) Node Spec

Copy this section once per node.

### Node: `<Node ID>`

- Type: `Agent | Mock API Workflow | Connector Activity | Flow Tool | Flow Control | Trigger`
- Business purpose:
- Flow node that invokes it:
- Inputs:
- Outputs:
- Upstream dependencies:
- Downstream dependencies:
- Exception cases:
- Auth/connection requirements:
- Demo fallback:

## 3) Mock API Workflow Notes

| API ID | Named System | Operation Name | Payload Path | Workflow JSON | Output Schema Source | Validation |
|---|---|---|---|---|---|---|
| API-001 |  |  | `apis/API-001/payload.json` | `apis/API-001/<WorkflowName>.json` | same payload source | Pending |

## 4) Flow Tool And Control Notes

| Node ID | Registry Node Type | Local Logic | Input Shape | Output Shape |
|---|---|---|---|---|
| TOOL-001 | `core.action.script` |  |  |  |

## 5) Connector Notes

| Connector ID | Connector Key | Activity | Required Connection | Required Fields |
|---|---|---|---|---|
| CONN-001 |  |  |  |  |

## 6) Out-Of-Scope Requests

| Request | Excluded Node/Product | Reason | Owner Decision |
|---|---|---|---|
|  | Live API call / RPA / Human Task / Case Management / Data Fabric / frontend | Outside deterministic demo scope |  |
