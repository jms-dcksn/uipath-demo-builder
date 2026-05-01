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
- Mock API Workflow projects shown:
- Script API placeholders shown:
- Agents shown:
- Connector activities shown:
- Flow tools shown:
- Flow control nodes shown:

## 3) Flow Topology

| Node ID | Label | Node Category | Registry Node Type | Purpose | Upstream | Downstream |
|---|---|---|---|---|---|---|
| FLOW-TRG-01 | Manual Trigger | Trigger | `core.trigger.manual` | Start demo run | None | TOOL-001 / API-001 |
| API-001 | ServiceNow Entitlement Lookup | API Workflow project / script placeholder | `uipath.core.api-workflow.<resourceKey>` or `core.action.script` | Return mock entitlement context | FLOW-TRG-01 | AG-001 / CTRL-001 |
| TOOL-001 | Normalize Payload | Flow Tool | `core.action.script` | Shape trigger payload for the agent | FLOW-TRG-01 / API-001 | AG-001 |
| AG-001 | Reason Over Request | Agent | `uipath.core.agent.<id>` or `uipath.agent.autonomous` | Classify, extract, summarize, or decide using documented payload paths | TOOL-001 / API-001 | CTRL-001 |
| CTRL-001 | Route Result | Flow Control | `core.logic.decision` | Route happy vs exception path | AG-001 | CONN-001 or FLOW-END-01 |

## 4) Operator Input Surface

Every field the demo operator must provide must be visible on the Start/manual trigger input form unless a different source is documented.

| Field | Fixture Key | Type | Required | Trigger Node ID | Flow Variable ID | Runtime Reference | Studio Web Visible? | Notes |
|---|---|---|---|---|---|---|---|---|
| documentExcerpt | documentExcerpt | string | Yes | start | documentExcerpt | `$vars.start.output.documentExcerpt` | Pending | Demo fixture input |

## 5) Variables And End Outputs

| Variable | Direction | Type | Set By | Consumed By | Notes |
|---|---|---|---|---|---|
| documentExcerpt | in | string | FLOW-TRG-01 | TOOL-001 / AG-001 | Must include `triggerNodeId: "start"` for manual-trigger demos |
| demoResult | out | object | FLOW-END-01 | Caller | Must be mapped on every reachable End node |

## 6) Branching Rules

| Rule ID | Location | Expression | True Path | False Path |
|---|---|---|---|---|
| CTRL-001 | Severity decision |  | Exception branch | Happy path |

## 7) Mock API Payload Consumption

Every row must point to a field documented in `discovery/payload-field-map.md`.

| API ID | Flow Node ID | Payload JSON Path | Runtime Reference | Consumed By | Purpose |
|---|---|---|---|---|---|
| API-001 |  | `$.entitlement.status` | `$vars.<apiNodeId>.output.entitlement.status` | CTRL-001 | Branch condition |
| API-001 |  | `$.entitlement.summary` | `$vars.<apiNodeId>.output.entitlement.summary` | AG-001 | Agent input |

## 8) API Workflow Artifact And Invocation Plan

| API ID | API Project Path | Solution Manifest Type | Upload `projectType` | Flow Invocation Mode | Flow Node ID | Notes |
|---|---|---|---|---|---|---|
| API-001 | `flow/<SolutionName>/<ApiProjectName>/` | `Api` | Pending `Api` | `bound API node` / `script placeholder` |  | Script placeholder is temporary until native binding is available |

## 9) Resource Plan

| Node ID | Node Family | Name | Discovery Method | Status | Notes |
|---|---|---|---|---|---|
| API-001 | API Workflow |  | project build + solution registration + in-solution or tenant registry | Planned | Mock passthrough API project; script placeholder if native binding is unavailable |
| AG-001 | Agent |  | tenant registry / local registry / scaffold | Planned |  |
| CONN-001 | Connector |  | Integration Service connection | Planned |  |
| TOOL-001 | Flow Tool |  | registry get | Planned |  |
| CTRL-001 | Flow Control |  | registry get | Planned |  |

## 10) Build Notes

- Solution-first scaffold check:
- API workflow project registration notes:
- API workflow invocation placeholder notes:
- Required definitions copied:
- Connector binding notes:
- Tool/control node notes:
- `=js:` mapping notes:
- End-node mapping notes:

## 11) Open Prerequisites

| ID | Prerequisite | Owner | Needed Before | Status |
|---|---|---|---|---|
| PRE-001 |  |  | Flow build | Open |
