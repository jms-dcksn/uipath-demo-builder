# Flow Node Contracts

## 1) Contract Index

| Contract ID | Flow Node ID | Node Type | Input Source | Output Shape | Downstream Consumer |
|---|---|---|---|---|---|
| AG-001 | classifyRequest | Agent | `$vars.start.output.<field>` or normalized trigger payload | `{ "classification": "...", "reason": "..." }` | CTRL-001 |

## 2) Operator Input Surface

| Field | Fixture Key | Flow Variable ID | Trigger Node ID | Required | Runtime Reference | Studio Web Visible? | Notes |
|---|---|---|---|---|---|---|---|
| documentExcerpt | documentExcerpt | documentExcerpt | start | Yes | `$vars.start.output.documentExcerpt` | Pending | Add `triggerNodeId: "start"` in `.flow` |

## 3) Agent Contracts

| Agent ID | Flow Node ID | Agent Source | Input Mapping | Output Mapping | Notes |
|---|---|---|---|---|---|
| AG-001 |  | existing / coded / low-code / inline |  | `$vars.<nodeId>.output.content` |  |

## 4) Connector Contracts

| Connector ID | Flow Node ID | Connector Key | Activity | Inputs | Outputs | Error Handling |
|---|---|---|---|---|---|---|
| CONN-001 |  |  |  |  | `$vars.<nodeId>.output` | route to exception |

## 5) Flow Tool Contracts

| Tool ID | Flow Node ID | Registry Node Type | Purpose | Inputs | Outputs |
|---|---|---|---|---|---|
| TOOL-001 |  | `core.action.script` | Normalize payload | trigger output | `$vars.<nodeId>.output` |

## 6) Flow Control Contracts

| Control ID | Flow Node ID | Registry Node Type | Logic | Inputs | Outputs / Ports |
|---|---|---|---|---|---|
| CTRL-001 |  | `core.logic.decision` |  | agent output | true / false |

## 7) Expression Register

Use `=js:` for every `$vars`, `$metadata`, or `$self` reference inside value fields.

| Location | Expression | Purpose |
|---|---|---|
| End output | `=js:$vars.<nodeId>.output` | Return final result |
| Manual-trigger input | `=js:$vars.start.output.<field>` | Pass operator-provided input to tool, agent, or connector |

## 8) End Outputs

| End Node | Output Variable | Source Expression | Covered Path |
|---|---|---|---|
| FLOW-END-01 | demoResult |  | happy |
