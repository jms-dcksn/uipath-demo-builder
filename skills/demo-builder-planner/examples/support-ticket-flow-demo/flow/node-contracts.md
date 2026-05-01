# Node Contracts - Support Ticket Flow

## Agent Contract

| Agent ID | Flow Node ID | Input Mapping | Output Mapping | Notes |
|---|---|---|---|---|
| AG-001 | classifyUrgency | normalized ticket object + `$vars.serviceNowEntitlementLookup.output.entitlement.summary` + `$vars.serviceNowEntitlementLookup.output.account.customerTier` | `$vars.classifyUrgency.output.content` | Expected JSON string with urgency, reason, nextAction |

## Mock API Workflow Contract

| API ID | Flow Node ID | Workflow Name | Registry Node Type | Output Reference | Fallback |
|---|---|---|---|---|---|
| API-001 | serviceNowEntitlementLookup | ServiceNowEntitlementLookup | `uipath.core.api-workflow.<servicenow-entitlement-resource>` | `$vars.serviceNowEntitlementLookup.output` | Fixture-backed Flow tool injection |

## Payload Field Consumption

| API ID | JSON Path | Runtime Reference | Consumed By | Consumer Type |
|---|---|---|---|---|
| API-001 | `$.account.accountId` | `$vars.serviceNowEntitlementLookup.output.account.accountId` | AG-001 / FLOW-END-01 | agent-input / end-output |
| API-001 | `$.entitlement.status` | `$vars.serviceNowEntitlementLookup.output.entitlement.status` | CTRL-001 | condition-input |
| API-001 | `$.entitlement.summary` | `$vars.serviceNowEntitlementLookup.output.entitlement.summary` | AG-001 / CONN-001 | agent-input / connector-input |

## Flow Tool Contracts

| Tool ID | Flow Node ID | Registry Node Type | Input Mapping | Output Mapping |
|---|---|---|---|---|
| TOOL-001 | normalizeTicket | `core.action.script` | trigger payload | `$vars.normalizeTicket.output` |

## Connector Contract

| Connector ID | Flow Node ID | Input Mapping | Output Mapping |
|---|---|---|---|
| CONN-001 | notifyTeam | ticket summary and enriched details | `$vars.notifyTeam.output` |

## Flow Control Contract

| Control ID | Flow Node ID | Registry Node Type | Input Mapping | Ports |
|---|---|---|---|---|
| CTRL-001 | urgencyBranch | `core.logic.decision` | parsed agent urgency or `$vars.serviceNowEntitlementLookup.output.entitlement.status !== "active"` | true -> exception End, false -> connector |

## End Output

| End Node | Output Variable | Source Expression | Covered Path |
|---|---|---|---|
| FLOW-END-01 | demoResult | `=js:$vars.notifyTeam.output` | happy |
| FLOW-END-01 | demoResult | `=js:{ status: "exception", reason: $vars.classifyUrgency.output.content }` | exception |
