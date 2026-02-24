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
- Decision policy:
- Guardrails:
- Human escalation condition:
- Observability signals:

## 3) Implementation Skeleton

Start from the generated scaffold (`uipath new <agent-name>`), then customize `main.py`.

```python
from langchain.agents import create_agent
from typing import Any

# Build from scaffolded project artifacts created by:
# uipath new <agent-name>
# uipath init
tools: list[Any] = []  # Add UiPath or custom tools required by the task

agent = create_agent(
    model="YOUR_MODEL_OR_PROVIDER_CONFIG",
    tools=tools,
    system_prompt="Define the role, boundaries, and output contract."
)
```

## 4) Runtime And Security

- Tenant/environment assumptions:
- Credential sources:
- Secrets handling:
- Data retention and redaction rules:

## 5) Validation Plan

| Test ID | Scenario | Expected Behavior | Pass Criteria |
|---|---|---|---|
| AG-T-01 | Missing required field | Agent flags invalid and routes for review | Structured error with reason code |
