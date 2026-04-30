# Agent Build Spec Template

Prefer `uipath-langchain` and `create_agent` for coded agents.
Every `AG-*` must map to one Flow node contract.
Do not multiplex multiple role/system prompts through one shared runtime.

## 1) Agent Index

| Agent ID | Agent Name | Mode | Project Path / Registry Name | Flow Node ID | Inputs | Outputs | Owner |
|---|---|---|---|---|---|---|---|
| AG-001 | `triage-agent` | coded | `agents/AG-001/triage-agent` | classifyRequest | normalized context | classification JSON |  |

## 2) Bootstrap Plan

| Agent ID | Command | Working Directory | Expected Output Folder | Status |
|---|---|---|---|---|
| AG-001 | `uv add uipath-langchain && uv sync && uip codedagent setup --output json && uip codedagent new triage-agent` | `builds/<demo-slug>/agents/AG-001` | `builds/<demo-slug>/agents/AG-001/triage-agent` | Planned |

## 3) Agent Specification

### Agent: `<Agent ID>`

- Agent source: `existing published | existing local sibling | coded | low-code | inline`
- Agent project/name:
- Flow node ID:
- Objective:
- Inputs from Flow:
- Outputs back to Flow:
- Prompt strategy:
- Tools:
- Context Grounding config, if provided: `index_name`, `folder_path`, retriever tool name.
- MCP config, if provided: `streamable_http_url`, auth method, enabled tool names.
- Mock/fallback tool strategy:
- Human escalation condition:

## 4) Tool Contract

| Tool Name | Type (`UiPath`/`API`/`Retriever`/`MCP`/`Mock`) | Input Contract | Output Contract | Runtime Source |
|---|---|---|---|---|
| `mock_policy_check` | Mock | `payload: str` | pass/fail + reason | Local deterministic stub |

## 5) Flow Contract

| Flow Mapping | Value |
|---|---|
| Agent node output source | `$vars.<nodeId>.output.content` |
| Parsed output shape |  |
| Downstream node(s) |  |
| Exception path |  |

## 6) Validation Plan

| Test ID | Scenario | Expected Behavior | Pass Criteria |
|---|---|---|---|
| AG-T-01 | Happy path input | Agent returns expected JSON | Output matches Flow contract |
| AG-T-02 | Exception input | Agent returns route/reason | Flow can branch on output |

## 7) Smoke Eval Artifact

- Eval path: `evaluations/eval-sets/smoke-test.json`
- Local run command:
  ```bash
  uip codedagent run <entrypoint> '<input-json>'
  ```
- Smoke eval command:
  ```bash
  uip codedagent eval <entrypoint> evaluations/eval-sets/smoke-test.json --no-report
  ```
