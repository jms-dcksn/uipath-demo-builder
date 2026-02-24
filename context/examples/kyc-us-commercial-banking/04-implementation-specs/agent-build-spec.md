# Agent Build Spec - KYC Onboarding

Prefer `uipath-langchain` and `create_agent` for agent implementation.

Reference: https://uipath.github.io/uipath-python/langchain/quick_start/

## 1) Agent Index

| Agent ID | Task IDs | Role | Inputs | Outputs | Owner |
|---|---|---|---|---|---|
| AG-COMP-01 | T-003 | Completeness validation | Extracted field set + confidence | Missing fields + readiness score | AI Team |
| AG-OWN-01 | T-005 | Ownership compliance analysis | Beneficial owner structure | Ownership pass/fail + rationale | AI Team |
| AG-BRIEF-01 | T-007 | Reviewer briefing synthesis | Screening output + case notes | Structured review brief | AI Team |

## 2) Agent Specification

### Agent: `AG-COMP-01`

- Objective: Determine whether a case can proceed to screening without missing critical information.
- Trigger/event: Completion of T-002 extraction.
- Inputs: Required-field policy, extracted fields, confidence metrics.
- Outputs: `isComplete`, `missingFields[]`, `confidenceFlags[]`, `recommendedStatus`.
- Tooling dependencies: Policy rules source, entity read/update tool.
- Decision policy: Deterministic required-field checks with explicit thresholds.
- Guardrails: No approval authority; cannot set final decision.
- Human escalation condition: Any critical field missing twice.
- Observability signals: completeness score distribution, false-negative rate.

### Agent: `AG-BRIEF-01`

- Objective: Summarize risk and evidence for fast, accountable human adjudication.
- Trigger/event: T-006 screening completed.
- Inputs: Screening hits, ownership profile, validation notes.
- Outputs: brief sections: `KeyFindings`, `RiskDrivers`, `RecommendedNextAction`.
- Tooling dependencies: Case read access, screening result parser.
- Decision policy: Summarization only; no autonomous approval/rejection.
- Guardrails: Must cite source task IDs for every risk statement.
- Human escalation condition: Any confirmed match or unresolved ambiguity.
- Observability signals: average briefing generation time, reviewer acceptance feedback.

## 3) Implementation Skeleton

Start from scaffolded project artifacts.

```python
from typing import Any
from langchain.agents import create_agent

# Scaffold commands:
# uipath new kyc-review-agent
# uipath init

tools: list[Any] = [
    # Add UiPath and custom tools used by agent tasks
]

agent = create_agent(
    model="YOUR_MODEL_OR_PROVIDER_CONFIG",
    tools=tools,
    system_prompt=(
        "You are a KYC support agent. "
        "Return structured outputs only. "
        "Do not make final approval or rejection decisions."
    )
)
```

## 4) Runtime And Security

- Tenant/environment assumptions: Separate dev/demo/prod style environments.
- Credential sources: Vault-managed secrets only.
- Secrets handling: No credentials in prompts or logs.
- Data retention and redaction rules: Mask PII in nonessential logs.

## 5) Validation Plan

| Test ID | Scenario | Expected Behavior | Pass Criteria |
|---|---|---|---|
| AG-T-01 | Missing required field | AG-COMP-01 flags incomplete and suggests PendingInfo | Structured output with missing field list |
| AG-T-02 | Ambiguous screening summary input | AG-BRIEF-01 highlights ambiguity and recommends manual adjudication | No autonomous decision language |
| AG-T-03 | Malformed owner payload | AG-OWN-01 returns validation error and escalation tag | Safe failure with reason code |
