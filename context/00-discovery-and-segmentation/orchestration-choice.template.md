# Orchestration Choice Template (BPMN vs Case Management)

## 1) Decision Context

- Use case:
- Decision owner:
- Date:

## 2) Scoring Matrix

Score each criterion from 1 (low) to 5 (high).

| Criterion | BPMN Score | Case Mgmt Score | Notes |
|---|---|---|---|
| Process predictability |  |  | |
| Need for strict sequencing |  |  | |
| Human discretion required |  |  | |
| Frequency of ad-hoc tasks |  |  | |
| Compliance/audit rigidity |  |  | |
| Case-centric collaboration |  |  | |
| Exception variability |  |  | |

## 3) Decision Rule

- Recommend `BPMN` when procedural flow, strict order, and automation autonomy dominate.
- Recommend `Case Management` when adaptive progression, collaboration, and human judgment dominate.

## 4) Recommendation

- Recommended model:
- Why:
- Risks of this choice:
- Mitigations:

## 5) Output Artifacts

- If BPMN: review `context/01-use-case-agentic-logic/example-bpmn.bpmn` as a comparison point for demo best practices.
- If BPMN: generate a Mermaid flow that represents the intended process structure.
- If BPMN: generate a new valid/importable BPMN skeleton (`.bpmn`) for user wiring in the proprietary web editor.
- If Case Management: generate stage/task lifecycle diagram plus task contracts.
