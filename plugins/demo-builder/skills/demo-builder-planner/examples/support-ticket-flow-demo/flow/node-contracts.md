# Node Contracts - Support Ticket Flow

## Agent Contract

| Agent ID | Flow Node ID | Input Mapping | Output Mapping | Notes |
|---|---|---|---|---|
| AG-001 | classifyUrgency | normalized ticket object | `$vars.classifyUrgency.output.content` | Expected JSON string with urgency, reason, nextAction |

## Flow Tool Contracts

| Tool ID | Flow Node ID | Registry Node Type | Input Mapping | Output Mapping |
|---|---|---|---|---|
| TOOL-001 | normalizeTicket | `core.action.script` | trigger payload | `$vars.normalizeTicket.output` |
| TOOL-002 | enrichTicket | `core.action.script` | `=js:$vars.normalizeTicket.output.accountId` | `$vars.enrichTicket.output` |

## Connector Contract

| Connector ID | Flow Node ID | Input Mapping | Output Mapping |
|---|---|---|---|
| CONN-001 | notifyTeam | ticket summary and enriched details | `$vars.notifyTeam.output` |

## Flow Control Contract

| Control ID | Flow Node ID | Registry Node Type | Input Mapping | Ports |
|---|---|---|---|---|
| CTRL-001 | urgencyBranch | `core.logic.decision` | parsed agent urgency | true -> exception End, false -> connector |

## End Output

| End Node | Output Variable | Source Expression | Covered Path |
|---|---|---|---|
| FLOW-END-01 | demoResult | `=js:$vars.notifyTeam.output` | happy |
| FLOW-END-01 | demoResult | `=js:{ status: "exception", reason: $vars.classifyUrgency.output.content }` | exception |
