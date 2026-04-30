# Flow Architecture

## 1) Demo Metadata

- Demo slug:
- Use case:
- Business goal:
- Target audience:
- Timebox:
- Flow solution name:
- Flow project name:

## 2) Demo Slice

- Happy path:
- Exception path:
- Systems shown:
- Agents shown:
- Connector activities shown:
- Flow tools shown:
- Flow control nodes shown:

## 3) Flow Topology

| Node ID | Label | Node Category | Registry Node Type | Purpose | Upstream | Downstream |
|---|---|---|---|---|---|---|
| FLOW-TRG-01 | Manual Trigger | Trigger | `core.trigger.manual` | Start demo run | None | FLOW-SCR-01 |
| TOOL-001 | Normalize Payload | Flow Tool | `core.action.script` | Shape trigger payload for the agent | FLOW-TRG-01 | AG-001 |
| AG-001 | Reason Over Request | Agent | `uipath.core.agent.<id>` or `uipath.agent.autonomous` | Classify, extract, summarize, or decide | TOOL-001 | CTRL-001 |
| CTRL-001 | Route Result | Flow Control | `core.logic.decision` | Route happy vs exception path | AG-001 | CONN-001 or FLOW-END-01 |

## 4) Trigger Contract

| Field | Type | Required | Example | Notes |
|---|---|---|---|---|
| requestId | string | Yes | DEMO-001 | Demo fixture key |

## 5) Variables And End Outputs

| Variable | Direction | Type | Set By | Consumed By | Notes |
|---|---|---|---|---|---|
| demoResult | out | object | FLOW-END-01 | Caller | Must be mapped on every reachable End node |

## 6) Branching Rules

| Rule ID | Location | Expression | True Path | False Path |
|---|---|---|---|---|
| CTRL-001 | Severity decision |  | Exception branch | Happy path |

## 7) Resource Plan

| Node ID | Node Family | Name | Discovery Method | Status | Notes |
|---|---|---|---|---|---|
| AG-001 | Agent |  | tenant registry / local registry / scaffold | Planned |  |
| CONN-001 | Connector |  | Integration Service connection | Planned |  |
| TOOL-001 | Flow Tool |  | registry get | Planned |  |
| CTRL-001 | Flow Control |  | registry get | Planned |  |

## 8) Build Notes

- Solution-first scaffold check:
- Required definitions copied:
- Connector binding notes:
- Tool/control node notes:
- `=js:` mapping notes:
- End-node mapping notes:

## 9) Open Prerequisites

| ID | Prerequisite | Owner | Needed Before | Status |
|---|---|---|---|---|
| PRE-001 |  |  | Flow build | Open |
