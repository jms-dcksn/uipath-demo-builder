# Orchestration Choice - BPMN vs Case Management

## 1) Decision Context

- Use case: US commercial banking KYC onboarding
- Decision owner: Demo architect + compliance SME
- Date: 2026-02-21

## 2) Scoring Matrix

| Criterion | BPMN Score | Case Mgmt Score | Notes |
|---|---|---|---|
| Process predictability | 3 | 4 | Main path exists but frequent rework/branching |
| Need for strict sequencing | 4 | 3 | Some steps are ordered, but not all |
| Human discretion required | 2 | 5 | Reviewer judgment is central |
| Frequency of ad-hoc tasks | 2 | 5 | Info requests and manual investigations are common |
| Compliance/audit rigidity | 4 | 4 | Both can satisfy with strong logging |
| Case-centric collaboration | 2 | 5 | Multi-role collaboration on single case context |
| Exception variability | 2 | 5 | Exception handling dominates real-world KYC |

## 3) Decision Rule

- Recommend `BPMN` when flow is mostly deterministic and straight-through processing dominates.
- Recommend `Case Management` when iterative investigation and discretionary work drive outcomes.

## 4) Recommendation

- Recommended model: `Case Management`
- Why: KYC onboarding requires iterative evidence gathering, exception handling, and accountable human adjudication.
- Risks of this choice: More configuration complexity in stage/task policies.
- Mitigations: Standardized stage/task templates and strict task contracts.

## 5) Output Artifacts

- Primary: case stage/task model + task contracts + case entity schema.
- Secondary BPMN comparison baseline: `context/01-use-case-agentic-logic/example-bpmn.bpmn`.
- Secondary BPMN flow artifact: Mermaid representation in `01-use-case-agentic-logic/maestro-design.md`.
- Secondary BPMN import artifact: `01-use-case-agentic-logic/kyc-onboarding-procedural-skeleton.bpmn`.
- BPMN skeleton scope: valid/importable process skeleton with placeholder tasks and routing; automation wiring deferred to web editor.
