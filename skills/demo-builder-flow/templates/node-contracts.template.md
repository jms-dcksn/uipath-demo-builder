# Flow Node Contracts

## 1) Contract Index

| Contract ID | Flow Node ID | Node Type | Input Source | Output Shape | Downstream Consumer |
|---|---|---|---|---|---|
| API-001 | serviceNowEntitlementLookup | API Workflow | manual trigger or default mock input | output schema from `apis/API-001/payload.json` | AG-001 / CTRL-001 |
| AG-001 | classifyRequest | Agent | `$vars.start.output.<field>` or normalized trigger payload | `{ "classification": "...", "reason": "..." }` | CTRL-001 |

## 2) Operator Input Surface

| Field | Fixture Key | Flow Variable ID | Trigger Node ID | Required | Runtime Reference | Studio Web Visible? | Notes |
|---|---|---|---|---|---|---|---|
| documentExcerpt | documentExcerpt | documentExcerpt | start | Yes | `$vars.start.output.documentExcerpt` | Pending | Add `triggerNodeId: "start"` in `.flow` |

## 3) Agent Contracts

| Agent ID | Flow Node ID | Agent Source | Input Mapping | Output Mapping | Notes |
|---|---|---|---|---|---|
| AG-001 |  | existing / coded / low-code / inline | Include documented API paths, not prose-only fields | `$vars.<nodeId>.output.content` |  |

## 4) Mock API Workflow Contracts

| API ID | Flow Node ID | Workflow Name | Registry Node Type | Payload Source | Output Reference | Fallback |
|---|---|---|---|---|---|---|
| API-001 |  |  | `uipath.core.api-workflow.<resourceKey>` | `apis/API-001/payload.json` | `$vars.<apiNodeId>.output` | Fixture-backed Flow tool injection |

## 5) Payload Field Consumption

Every JSON path must exist in `discovery/payload-field-map.md`.

| API ID | JSON Path | Runtime Reference | Consumed By | Consumer Type | Expected Values |
|---|---|---|---|---|---|
| API-001 | `$.entitlement.status` | `$vars.<apiNodeId>.output.entitlement.status` | CTRL-001 | condition-input | `active`, `inactive`, `suspended` |
| API-001 | `$.entitlement.summary` | `$vars.<apiNodeId>.output.entitlement.summary` | AG-001 | agent-input | string |

## 6) Connector Contracts

| Connector ID | Flow Node ID | Connector Key | Activity | Inputs | Outputs | Error Handling |
|---|---|---|---|---|---|---|
| CONN-001 |  |  |  |  | `$vars.<nodeId>.output` | route to exception |

## 7) Flow Tool Contracts

| Tool ID | Flow Node ID | Registry Node Type | Purpose | Inputs | Outputs |
|---|---|---|---|---|---|
| TOOL-001 |  | `core.action.script` | Normalize payload | trigger output | `$vars.<nodeId>.output` |

## 8) Flow Control Contracts

| Control ID | Flow Node ID | Registry Node Type | Logic | Inputs | Outputs / Ports |
|---|---|---|---|---|---|
| CTRL-001 |  | `core.logic.decision` |  | agent output or documented API payload path | true / false |

## 9) Expression Register

Use `=js:` for every `$vars`, `$metadata`, or `$self` reference inside value fields.

| Location | Expression | Purpose |
|---|---|---|
| End output | `=js:$vars.<nodeId>.output` | Return final result |
| Manual-trigger input | `=js:$vars.start.output.<field>` | Pass operator-provided input to tool, agent, or connector |
| API payload field | `=js:$vars.<apiNodeId>.output.entitlement.status` | Pass documented mock API field to agent, condition, connector, or End output |

## 10) End Outputs

| End Node | Output Variable | Source Expression | Covered Path |
|---|---|---|---|
| FLOW-END-01 | demoResult |  | happy |
