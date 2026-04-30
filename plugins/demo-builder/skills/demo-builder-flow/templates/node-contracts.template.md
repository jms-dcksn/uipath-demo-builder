# Flow Node Contracts

## 1) Contract Index

| Contract ID | Flow Node ID | Node Type | Input Source | Output Shape | Downstream Consumer |
|---|---|---|---|---|---|
| AG-001 | classifyRequest | Agent | trigger payload | `{ "classification": "...", "reason": "..." }` | CTRL-001 |

## 2) Agent Contracts

| Agent ID | Flow Node ID | Agent Source | Input Mapping | Output Mapping | Notes |
|---|---|---|---|---|---|
| AG-001 |  | existing / coded / low-code / inline |  | `$vars.<nodeId>.output.content` |  |

## 3) Connector Contracts

| Connector ID | Flow Node ID | Connector Key | Activity | Inputs | Outputs | Error Handling |
|---|---|---|---|---|---|---|
| CONN-001 |  |  |  |  | `$vars.<nodeId>.output` | route to exception |

## 4) Flow Tool Contracts

| Tool ID | Flow Node ID | Registry Node Type | Purpose | Inputs | Outputs |
|---|---|---|---|---|---|
| TOOL-001 |  | `core.action.script` | Normalize payload | trigger output | `$vars.<nodeId>.output` |

## 5) Flow Control Contracts

| Control ID | Flow Node ID | Registry Node Type | Logic | Inputs | Outputs / Ports |
|---|---|---|---|---|---|
| CTRL-001 |  | `core.logic.decision` |  | agent output | true / false |

## 6) Expression Register

Use `=js:` for every `$vars`, `$metadata`, or `$self` reference inside value fields.

| Location | Expression | Purpose |
|---|---|---|
| End output | `=js:$vars.<nodeId>.output` | Return final result |

## 7) End Outputs

| End Node | Output Variable | Source Expression | Covered Path |
|---|---|---|---|
| FLOW-END-01 | demoResult |  | happy |
