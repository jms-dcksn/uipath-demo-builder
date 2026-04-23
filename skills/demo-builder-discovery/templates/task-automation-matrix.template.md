# Task Automation Matrix Template

Map each segment to tasks and execution technologies.

## 1) Task Matrix

| Segment ID | Task ID | Task Name | Purpose | Execution Type | Component | Input | Output | Exception Path |
|---|---|---|---|---|---|---|---|---|
| SEG-01 | T-001 | Extract submission data | Convert documents to structured data | Automated | IDP | Application docs | Extracted fields | Route to manual validation |
| SEG-01 | T-002 | Validate extracted data | Check completeness and consistency | Automated | AI Agent | Extracted fields | Validation flags | Create review task |
| SEG-02 | T-003 | Update system of record | Persist validated data | Automated | API | Structured payload | Update confirmation | Queue retry / escalate |
| SEG-03 | T-004 | Eligibility review | Make policy decision | Human-in-loop | Human Task + Agent assist | Case summary | Decision + reason | Escalate to supervisor |

## 2) Task Dependencies

| Task ID | Depends On | Dependency Type |
|---|---|---|
| T-002 | T-001 | Data prerequisite |

## 3) Automation Fit Notes

- `AI Agent` when reasoning/summarization/classification is needed.
- `IDP` when extraction from documents is primary.
- `RPA` when UI-based legacy system interactions are required.
- `API` when stable service interfaces exist.
- `Human Task` when policy/accountability requires human decision.

## 4) Task-Level Non-Functional Targets

| Task ID | SLA | Accuracy Target | Audit Requirement |
|---|---|---|---|
| T-001 | 2 min | >= 95% critical field capture | Store extraction confidence by field |

