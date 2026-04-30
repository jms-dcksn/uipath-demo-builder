# Flow Architecture - Support Ticket Flow

## Demo Slice

- Happy path: normal-priority ticket is normalized, classified, enriched from fixture-safe context, sent to the support team, and returned as a final result.
- Exception path: high-urgency ticket is routed by control logic to a clear exception result after agent classification.

## Flow Topology

| Node ID | Label | Node Category | Registry Node Type | Purpose | Upstream | Downstream |
|---|---|---|---|---|---|---|
| FLOW-TRG-01 | Ticket Trigger | Trigger | `core.trigger.manual` | Start demo run | None | TOOL-001 |
| TOOL-001 | Normalize Ticket | Flow Tool | `core.action.script` | Normalize trigger payload | FLOW-TRG-01 | AG-001 |
| AG-001 | Classify Urgency | Agent | `uipath.core.agent.<ticket-triage-agent>` | Classify ticket | TOOL-001 | TOOL-002 |
| TOOL-002 | Enrich Ticket | Flow Tool | `core.action.script` | Add fixture-backed entitlement detail | AG-001 | CTRL-001 |
| CTRL-001 | Urgency Branch | Flow Control | `core.logic.decision` | Route high urgency | TOOL-002 | CONN-001 or FLOW-END-01 |
| CONN-001 | Notify Team | Connector Activity | `uipath.connector.<service>.<activity>` | Send notification | CTRL-001 | FLOW-END-01 |
| FLOW-END-01 | Return Result | Flow Control | `core.control.end` | Return `demoResult` | CTRL-001 or CONN-001 | None |

## Variables

| Variable | Direction | Type | Set By | Notes |
|---|---|---|---|---|
| demoResult | out | object | FLOW-END-01 | Mapped on every reachable End node |
