# Mock API Payload Research - Support Ticket Flow

| API ID | Named System | Operation Name | Payload Convention | Selected Fields |
|---|---|---|---|---|
| API-001 | ServiceNow | `checkEntitlement` | Account entitlement lookup with case context | account identity, entitlement status, SLA, recent interaction |

## Field Classes

| Field Class | JSON Paths | Purpose |
|---|---|---|
| Identity | `$.account.accountId`, `$.caseContext.caseId` | Trace the support account and related case |
| Decision | `$.entitlement.status`, `$.entitlement.slaHours`, `$.caseContext.recommendedRoute` | Drive branch logic and routing |
| Narrative | `$.entitlement.summary`, `$.caseContext.recentInteraction` | Give AG-001 useful business context |
