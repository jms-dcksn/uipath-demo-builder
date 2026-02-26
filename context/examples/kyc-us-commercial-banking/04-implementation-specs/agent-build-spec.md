# Agent Build Spec - KYC Onboarding

Prefer `uipath-langchain` and `create_agent` for agent implementation.
Each identified agent is scaffolded independently with `uipath new <agent-name>`.
No multi-role prompt multiplexing is used in a shared runtime.

Reference: https://uipath.github.io/uipath-python/langchain/quick_start/

## 1) Agent Index

| Agent ID | Agent Name (`uipath new`) | Project Path | Task IDs | Role | Inputs | Outputs | Owner |
|---|---|---|---|---|---|---|---|
| AG-COMP-01 | `kyc-completeness-agent` | `agents/kyc-completeness-agent` | T-003 | Completeness validation | Extracted field set + confidence | Missing fields + readiness score | AI Team |
| AG-OWN-01 | `kyc-ownership-agent` | `agents/kyc-ownership-agent` | T-005 | Ownership compliance analysis | Beneficial owner structure | Ownership pass/fail + rationale | AI Team |
| AG-BRIEF-01 | `kyc-review-briefing-agent` | `agents/kyc-review-briefing-agent` | T-007 | Reviewer briefing synthesis | Screening output + case notes | Structured review brief | AI Team |

## 2) Bootstrap Plan (Required)

| Agent ID | Command | Working Directory | Expected Output Folder | Status |
|---|---|---|---|---|
| AG-COMP-01 | `uipath new kyc-completeness-agent` | `<repo-root>/agents` | `agents/kyc-completeness-agent` | Planned |
| AG-OWN-01 | `uipath new kyc-ownership-agent` | `<repo-root>/agents` | `agents/kyc-ownership-agent` | Planned |
| AG-BRIEF-01 | `uipath new kyc-review-briefing-agent` | `<repo-root>/agents` | `agents/kyc-review-briefing-agent` | Planned |

- A separate scaffold is created for each `AG-*` ID.
- Shared helper modules are permitted, but each project maintains its own prompt contract and tool list.

## 3) Agent Specification

### Agent: `AG-COMP-01`

- Agent project name (`uipath new <agent-name>`): `kyc-completeness-agent`.
- Project path: `agents/kyc-completeness-agent`.
- Objective: Determine whether a case can proceed to screening without missing critical information.
- Trigger/event: Completion of T-002 extraction.
- Inputs: Required-field policy, extracted fields, confidence metrics.
- Outputs: `isComplete`, `missingFields[]`, `confidenceFlags[]`, `recommendedStatus`.
- Tooling dependencies: Policy rules source, entity read/update tool.
- Architecture pattern: Single-role validation agent.
- Prompt strategy: Deterministic validation contract; no autonomous adjudication.
- Decision policy: Deterministic required-field checks with explicit thresholds.
- Guardrails: No approval authority; cannot set final decision.
- Human escalation condition: Any critical field missing twice.
- Observability signals: completeness score distribution, false-negative rate.
- Inter-agent handoff contract: Emits validated payload for AG-OWN-01 only when `isComplete=true`.
- Context Grounding config (if provided): `index_name=TBD`, `folder_path=TBD`, retriever tool name `kyc_completeness_context`.
- MCP config (if provided): `streamable_http_url=TBD`, auth `TBD`, enabled tools `TBD`.

### Agent: `AG-OWN-01`

- Agent project name (`uipath new <agent-name>`): `kyc-ownership-agent`.
- Project path: `agents/kyc-ownership-agent`.
- Objective: Assess beneficial ownership compliance against threshold and data-quality policies.
- Trigger/event: AG-COMP-01 returns complete payload.
- Inputs: Beneficial owner list, ownership percentages, policy thresholds.
- Outputs: `ownershipPass`, `violations[]`, `rationale`, `recommendedStatus`.
- Tooling dependencies: Ownership policy retriever, entity read/update tool.
- Architecture pattern: Single-role ownership rules agent.
- Prompt strategy: Policy-grounded analysis with structured JSON output.
- Decision policy: Rule-based threshold checks with deterministic fail reasons.
- Guardrails: No final approve/reject; route policy exceptions to human review.
- Human escalation condition: Any threshold breach or unresolved ownership ambiguity.
- Observability signals: violation frequency, manual override rate.
- Inter-agent handoff contract: Emits ownership assessment used by AG-BRIEF-01 for reviewer context.
- Context Grounding config (if provided): `index_name=TBD`, `folder_path=TBD`, retriever tool name `kyc_ownership_context`.
- MCP config (if provided): `streamable_http_url=TBD`, auth `TBD`, enabled tools `TBD`.

### Agent: `AG-BRIEF-01`

- Agent project name (`uipath new <agent-name>`): `kyc-review-briefing-agent`.
- Project path: `agents/kyc-review-briefing-agent`.
- Objective: Summarize risk and evidence for fast, accountable human adjudication.
- Trigger/event: T-006 screening completed.
- Inputs: Screening hits, ownership profile, validation notes.
- Outputs: brief sections: `KeyFindings`, `RiskDrivers`, `RecommendedNextAction`.
- Tooling dependencies: Case read access, screening result parser.
- Architecture pattern: Single-role summarization agent.
- Prompt strategy: Citation-first synthesis with no autonomous final decision.
- Decision policy: Summarization only; no autonomous approval/rejection.
- Guardrails: Must cite source task IDs for every risk statement.
- Human escalation condition: Any confirmed match or unresolved ambiguity.
- Observability signals: average briefing generation time, reviewer acceptance feedback.
- Inter-agent handoff contract: Final structured brief is persisted for the human compliance task.
- Context Grounding config (if provided): `index_name=TBD`, `folder_path=TBD`, retriever tool name `kyc_briefing_context`.
- MCP config (if provided): `streamable_http_url=TBD`, auth `TBD`, enabled tools `TBD`.

## 4) Tool Contract

| Tool Name | Type (`UiPath`/`API`/`Retriever`/`MCP`/`Mock`) | Task IDs | Input Contract | Output Contract | Runtime Source |
|---|---|---|---|---|---|
| `read_case_snapshot` | UiPath | T-003, T-005, T-007 | `case_id: str` | `dict` case snapshot | Data Fabric entity |
| `write_case_assessment` | UiPath | T-003, T-005, T-007 | assessment payload | persisted assessment status | Data Fabric entity |
| `kyc_policy_context_retriever` | Retriever | T-003, T-005 | `query: str` | KYC policy excerpts + citations | UiPath Context Grounding (`index_name`, `folder_path`) |
| `mcp_watchlist_enrichment` | MCP | T-007 | normalized subject payload | watchlist enrichment summary | Streamable HTTP MCP endpoint |
| `lookup_ownership_policy` | Retriever | T-005 | `policy_version: str` | ownership policy clauses | Policy KB |
| `mock_screening_summary_parser` | Mock | T-007 | screening payload | normalized findings list | Local deterministic stub |

## 5) Implementation Skeleton

Start from each scaffolded project artifact independently.

```python
from typing import Any
from langchain.agents import create_agent
from langchain_core.tools.retriever import create_retriever_tool
from uipath_langchain.retrievers import ContextGroundingRetriever

# Scaffold command is run once per agent:
# uipath new <agent-name>
# uipath init

def build_context_tool(index_name: str, folder_path: str) -> Any:
    retriever = ContextGroundingRetriever(index_name=index_name, folder_path=folder_path)
    return create_retriever_tool(
        retriever,
        "kyc_policy_context_retriever",
        "Retrieve KYC policy context and citations."
    )

def build_mcp_tools(mcp_url: str) -> list[Any]:
    # Bind streamable HTTP MCP tools when URL is provided.
    return []

tools: list[Any] = [
    # Add only tools required by this agent's mapped tasks
]
# tools.append(build_context_tool(index_name="YOUR_INDEX", folder_path="YOUR/FOLDER"))
# tools.extend(build_mcp_tools(mcp_url="https://example.com/mcp"))

agent = create_agent(
    model="YOUR_MODEL_OR_PROVIDER_CONFIG",
    tools=tools,
    system_prompt=(
        "You are the AG-<ROLE>-01 KYC agent for one specific role. "
        "Return structured outputs only. "
        "Do not make final approval or rejection decisions."
    )
)
```

## 6) Runtime And Security

- Tenant/environment assumptions: Separate dev/demo/prod style environments.
- Credential sources: Vault-managed secrets only.
- Secrets handling: No credentials in prompts or logs.
- Data retention and redaction rules: Mask PII in nonessential logs.

## 7) Validation Plan

| Test ID | Scenario | Expected Behavior | Pass Criteria |
|---|---|---|---|
| AG-T-01 | Missing required field | AG-COMP-01 flags incomplete and suggests PendingInfo | Structured output with missing field list |
| AG-T-02 | Ambiguous screening summary input | AG-BRIEF-01 highlights ambiguity and recommends manual adjudication | No autonomous decision language |
| AG-T-03 | Malformed owner payload | AG-OWN-01 returns validation error and escalation tag | Safe failure with reason code |
| AG-T-04 | Prompt multiplexing attempt | Any agent rejects/ignores request to switch to another agent role | Output remains within that agent's declared contract |
| AG-T-05 | Context Grounding configured | Retriever uses configured `index_name` + `folder_path` and returns citation-ready context | Retrieval output includes source references |
| AG-T-06 | MCP URL configured | MCP tools are invoked through streamable HTTP and mapped to output contract | Agent output includes MCP-enriched fields |
