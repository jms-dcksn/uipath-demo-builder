# Case Management Design Template

Use this template when adaptive, case-centric orchestration is preferred.
The final executable model is built in proprietary tooling; this document provides the build-ready blueprint.

## 1) Case Type Definition

- Case type name:
- Business objective:
- Primary case owner role:

## 2) Lifecycle Model

| Stage | Entry Criteria | Typical Tasks | Exit Criteria |
|---|---|---|---|
| Intake | Case created | Validate, enrich | Case is complete and eligible |

## 3) Milestones

| Milestone | Business Meaning | Trigger |
|---|---|---|
| Case triaged | Ready for review path selection | Triage decision recorded |

## 4) Task Templates

| Task Template | Assigned Role | Due In | Completion Rule |
|---|---|---|---|
| Review case package | Case reviewer | 8h | Decision and reason captured |

## 5) Escalation Model

- SLA thresholds:
- Escalation recipients:
- Escalation actions:

## 6) Collaboration And Notes

- Required comments:
- Attachments required:
- Audit trail expectations:

## 7) Exceptions

- Missing data exception:
- Policy conflict exception:
- External dependency exception:

## 8) Mermaid Stage/Task Blueprint

```mermaid
flowchart LR
    S1[Stage 1: Intake]
    S2[Stage 2: Processing]
    S3[Stage 3: Review]
    S4[Stage 4: Decision]

    S1 --> S2 --> S3 --> S4

    S1T1([T-001 Extract Data (IDP)]) --> S1T2([T-002 Validate Data (AI Agent)])
    S2T1([T-003 System Update (API/RPA)])
    S3T1([T-004 Eligibility Review (Human Task)])
    S4T1([T-005 Final Decision])

    S1 --- S1T1
    S1 --- S1T2
    S2 --- S2T1
    S3 --- S3T1
    S4 --- S4T1
```

## 9) Proprietary Model Handoff Notes

- Stage names to implement:
- Task templates to implement:
- Data fields required by each task:
- SLA/escalation settings:
- Manual configuration checklist:
