# Agent Build Spec Template (UiPath Python + LangChain)

Prefer `uipath-langchain` and `create_agent` for agent implementation.
Every identified agent must be scaffolded as an independent project with `uipath new <agent-name>`.
Do not multiplex multiple role/system prompts through one shared agent runtime.

## References

- `https://uipath.github.io/uipath-python/langchain/quick_start/`

## 1) Agent Index

| Agent ID | Agent Name (`uipath new`) | Project Path | Task IDs | Role | Inputs | Outputs | Owner |
|---|---|---|---|---|---|---|---|
| AG-VAL-01 | `validation-agent` | `agents/validation-agent` | T-002 | Data validation and anomaly detection | Extracted fields | Validation result + explanation |  |

## 2) Bootstrap Plan (Required)

| Agent ID | Command | Working Directory | Expected Output Folder | Status |
|---|---|---|---|---|
| AG-VAL-01 | `uipath new validation-agent` | `<repo-root>/agents` | `agents/validation-agent` | Planned |

- Run one bootstrap command per row in the Agent Index.
- Do not build a shared router agent that switches between different role prompts.
- Shared libraries are allowed, but each scaffolded agent must have its own `main.py`, system prompt, and task-aligned tool list.

## 3) Agent Specification

### Agent: `<Agent ID>`

- Agent project name (`uipath new <agent-name>`):
- Project path:
- Objective:
- Trigger/event:
- Inputs:
- Outputs:
- Tooling dependencies:
- Architecture pattern (single-agent, planner-worker, reviewer, etc.; if multi-agent, list all participating `AG-*` IDs):
- Prompt strategy (single-role contract for this agent only):
- Decision policy:
- Guardrails:
- Human escalation condition:
- Observability signals:
- Inter-agent handoff contract (if applicable):
- Tool selection rationale (why these tools for these tasks):
- Mock/fallback tool strategy (when real integration is unavailable):

## 4) Tool Contract

| Tool Name | Type (`UiPath`/`API`/`Retriever`/`Mock`) | Task IDs | Input Contract | Output Contract | Runtime Source |
|---|---|---|---|---|---|
| `lookup_customer_profile` | API | T-001, T-003 | `customer_id: str` | `dict` with profile fields | CRM API |
| `mock_policy_check` | Mock | T-004 | `payload: str` | pass/fail + reason | Local deterministic stub |

## 5) Implementation Skeleton

Start from each generated scaffold (`uipath new <agent-name>`), then customize that project's `main.py` with real and/or mock tools needed for the mapped tasks.

```python
from langchain.agents import create_agent
from langchain.tools import tool
from typing import Any

# Build from one scaffolded project artifact set per agent:
# uipath new <agent-name>
# uipath init

@tool
def mock_policy_check(payload: str) -> str:
    """Deterministic demo stub; replace with real policy API."""
    text = payload.lower()
    if "high_risk" in text:
        return "FAIL: route_to_manual_review"
    return "PASS: auto_approve"

tools: list[Any] = [mock_policy_check]  # Add only tools this specific agent needs

agent = create_agent(
    model="YOUR_MODEL_OR_PROVIDER_CONFIG",
    tools=tools,
    system_prompt="Define one role, boundaries, and output contract for this agent."
)
```

## 6) Runtime And Security

- Tenant/environment assumptions:
- Credential sources:
- Secrets handling:
- Data retention and redaction rules:

## 7) Validation Plan

| Test ID | Scenario | Expected Behavior | Pass Criteria |
|---|---|---|---|
| AG-T-01 | Missing required field | Agent flags invalid and routes for review | Structured error with reason code |
| AG-T-02 | Mock tool path | Agent uses mock tool and returns deterministic output | Output matches expected predefined response |
| AG-T-03 | Prompt multiplexing attempt | Agent keeps role boundaries and does not switch to another agent role | Output stays within this agent's contract |
