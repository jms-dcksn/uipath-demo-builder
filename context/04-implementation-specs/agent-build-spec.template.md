# Agent Build Spec Template (UiPath Python + LangChain)

Prefer `uipath-langchain` and `create_agent` for agent implementation.

## References

- `https://uipath.github.io/uipath-python/langchain/quick_start/`

## 1) Agent Index

| Agent ID | Task IDs | Role | Inputs | Outputs | Owner |
|---|---|---|---|---|---|
| AG-VAL-01 | T-002 | Data validation and anomaly detection | Extracted fields | Validation result + explanation |  |

## 2) Agent Specification

### Agent: `<Agent ID>`

- Objective:
- Trigger/event:
- Inputs:
- Outputs:
- Tooling dependencies:
- Architecture pattern (single-agent, planner-worker, reviewer, etc.):
- Prompt strategy:
- Decision policy:
- Guardrails:
- Human escalation condition:
- Observability signals:
- Tool selection rationale (why these tools for these tasks):
- Mock/fallback tool strategy (when real integration is unavailable):

## 3) Tool Contract

| Tool Name | Type (`UiPath`/`API`/`Retriever`/`Mock`) | Task IDs | Input Contract | Output Contract | Runtime Source |
|---|---|---|---|---|---|
| `lookup_customer_profile` | API | T-001, T-003 | `customer_id: str` | `dict` with profile fields | CRM API |
| `mock_policy_check` | Mock | T-004 | `payload: str` | pass/fail + reason | Local deterministic stub |

## 4) Implementation Skeleton

Start from the generated scaffold (`uipath new <agent-name>`), then customize `main.py` with real and/or mock tools as needed for demo completeness.

```python
from langchain.agents import create_agent
from langchain.tools import tool
from typing import Any

# Build from scaffolded project artifacts created by:
# uipath new <agent-name>
# uipath init

@tool
def mock_policy_check(payload: str) -> str:
    """Deterministic demo stub; replace with real policy API."""
    text = payload.lower()
    if "high_risk" in text:
        return "FAIL: route_to_manual_review"
    return "PASS: auto_approve"

tools: list[Any] = [mock_policy_check]  # Add UiPath and custom tools required by the task

agent = create_agent(
    model="YOUR_MODEL_OR_PROVIDER_CONFIG",
    tools=tools,
    system_prompt="Define the role, boundaries, and output contract."
)
```

## 5) Runtime And Security

- Tenant/environment assumptions:
- Credential sources:
- Secrets handling:
- Data retention and redaction rules:

## 6) Validation Plan

| Test ID | Scenario | Expected Behavior | Pass Criteria |
|---|---|---|---|
| AG-T-01 | Missing required field | Agent flags invalid and routes for review | Structured error with reason code |
| AG-T-02 | Mock tool path | Agent uses mock tool and returns deterministic output | Output matches expected predefined response |
