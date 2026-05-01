# Mock API Payload Research

Document payload conventions for deterministic mock API workflows.

## 1) API Workflow Candidates

| API ID | Named System | Operation Name | Payload Source IDs | Selected? | Reason |
|---|---|---|---|---|---|
| API-001 | ServiceNow | `checkEntitlement` | SRC-API-001 | Yes | Supports entitlement-driven triage |

## 2) Payload Shape Notes

| API ID | Field Group | Convention / Source Note | Selected Fields |
|---|---|---|---|
| API-001 | Identity | Account and case identifiers support traceability | `account.accountId`, `caseContext.caseId` |
| API-001 | Decision | Entitlement status and SLA drive branch logic | `entitlement.status`, `entitlement.slaHours` |
| API-001 | Narrative | Summary and recent interaction feed agent reasoning | `entitlement.summary`, `caseContext.recentInteraction` |

## 3) Demo-Safe Values

| API ID | Field | Example Value | Rationale | Safety Check |
|---|---|---|---|---|
| API-001 | `$.account.accountId` | `ACC-100245` | Mock traceable identifier | Not real account data |

## 4) Source Gaps And Assumptions

| API ID | Gap | Assumption | Impact | Owner |
|---|---|---|---|---|
| API-001 |  |  |  |  |
