# Agent Build Spec Template (UiPath Python + LangChain)

Prefer `uipath-langchain` and `create_agent` for agent implementation.
Every identified agent must be scaffolded as an independent project with `uipath new <agent-name>`.
Do not multiplex multiple role/system prompts through one shared agent runtime.
When the user provides Context Grounding details, use the `bootstrap-uipath-agent` skill pattern and include both `index_name` and `folder_path`.
When the user provides an MCP URL, integrate streamable HTTP MCP tools for that agent.

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
- Context Grounding config (if provided): `index_name`, `folder_path`, retriever tool name.
- MCP config (if provided): `streamable_http_url`, auth method, enabled tool names.
- Tool selection rationale (why these tools for these tasks):
- Mock/fallback tool strategy (when real integration is unavailable):

## 4) Tool Contract

| Tool Name | Type (`UiPath`/`API`/`Retriever`/`MCP`/`Mock`) | Task IDs | Input Contract | Output Contract | Runtime Source |
|---|---|---|---|---|---|
| `lookup_customer_profile` | API | T-001, T-003 | `customer_id: str` | `dict` with profile fields | CRM API |
| `policy_context_retriever` | Retriever | T-002 | `query: str` | policy passages + citations | UiPath Context Grounding (`index_name`, `folder_path`) |
| `mcp_case_enrichment` | MCP | T-005 | structured request payload | enriched case signals | Streamable HTTP MCP endpoint |
| `mock_policy_check` | Mock | T-004 | `payload: str` | pass/fail + reason | Local deterministic stub |

## 5) Implementation Skeleton

Start from each generated scaffold (`uipath new <agent-name>`), then customize that project's `main.py` with real and/or mock tools needed for the mapped tasks.

```python
from langchain.agents import create_agent
from langchain_core.tools.retriever import create_retriever_tool
from langchain.tools import tool
from typing import Any
from uipath_langchain.retrievers import ContextGroundingRetriever

# Build from one scaffolded project artifact set per agent:
# uipath new <agent-name>
# uipath init

def build_context_tool(index_name: str, folder_path: str) -> Any:
    retriever = ContextGroundingRetriever(index_name=index_name, folder_path=folder_path)
    return create_retriever_tool(
        retriever,
        "policy_context_retriever",
        "Search approved internal context and return citations."
    )

def build_mcp_tools(mcp_url: str) -> list[Any]:
    # Bind streamable HTTP MCP tools for this agent when URL is provided.
    # Keep tool interfaces stable so mock replacements are low-friction.
    return []

@tool
def mock_policy_check(payload: str) -> str:
    """Deterministic demo stub; replace with real policy API."""
    text = payload.lower()
    if "high_risk" in text:
        return "FAIL: route_to_manual_review"
    return "PASS: auto_approve"

tools: list[Any] = [mock_policy_check]  # Add only tools this specific agent needs
# tools.append(build_context_tool(index_name="YOUR_INDEX", folder_path="YOUR/FOLDER"))
# tools.extend(build_mcp_tools(mcp_url="https://example.com/mcp"))

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
| AG-T-04 | Context Grounding configured | Retriever uses configured `index_name` + `folder_path` and returns citations | Tool output includes citation-ready context |
| AG-T-05 | MCP URL configured | Agent invokes MCP-backed tools and handles endpoint response | MCP tool output is mapped into agent contract |
