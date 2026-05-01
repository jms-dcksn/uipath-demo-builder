# Flow Architecture - Support Ticket Flow

## Demo Slice

- Happy path: normal-priority ticket is normalized, classified, enriched from fixture-safe context, sent to the support team, and returned as a final result.
- Exception path: high-urgency ticket or inactive entitlement is routed by control logic to a clear exception result after agent classification.

## Flow Topology

| Node ID | Label | Node Category | Registry Node Type | Purpose | Upstream | Downstream |
|---|---|---|---|---|---|---|
| FLOW-TRG-01 | Ticket Trigger | Trigger | `core.trigger.manual` | Start demo run | None | TOOL-001 |
| TOOL-001 | Normalize Ticket | Flow Tool | `core.action.script` | Normalize trigger payload | FLOW-TRG-01 | AG-001 |
| API-001 | ServiceNow Entitlement Lookup | API Workflow | `uipath.core.api-workflow.<servicenow-entitlement-resource>` | Return mock support entitlement context | FLOW-TRG-01 | AG-001 / CTRL-001 |
| AG-001 | Classify Urgency | Agent | `uipath.core.agent.<ticket-triage-agent>` | Classify ticket using ticket and entitlement context | TOOL-001 / API-001 | CTRL-001 |
| CTRL-001 | Urgency Or Entitlement Branch | Flow Control | `core.logic.decision` | Route high urgency or inactive entitlement | AG-001 / API-001 | CONN-001 or FLOW-END-01 |
| CONN-001 | Notify Team | Connector Activity | `uipath.connector.<service>.<activity>` | Send notification | CTRL-001 | FLOW-END-01 |
| FLOW-END-01 | Return Result | Flow Control | `core.control.end` | Return `demoResult` | CTRL-001 or CONN-001 | None |

## Mock API Payload Consumption

| API ID | Flow Node ID | Payload JSON Path | Runtime Reference | Consumed By | Purpose |
|---|---|---|---|---|---|
| API-001 | serviceNowEntitlementLookup | `$.account.accountId` | `$vars.serviceNowEntitlementLookup.output.account.accountId` | AG-001 / FLOW-END-01 | Trace account context |
| API-001 | serviceNowEntitlementLookup | `$.entitlement.status` | `$vars.serviceNowEntitlementLookup.output.entitlement.status` | CTRL-001 | Branch if inactive or suspended |
| API-001 | serviceNowEntitlementLookup | `$.entitlement.summary` | `$vars.serviceNowEntitlementLookup.output.entitlement.summary` | AG-001 / CONN-001 | Explain support entitlement |

## Variables

| Variable | Direction | Type | Set By | Notes |
|---|---|---|---|---|
| demoResult | out | object | FLOW-END-01 | Mapped on every reachable End node |
