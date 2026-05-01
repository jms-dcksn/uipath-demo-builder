# System Interactions - Support Ticket Flow

| Interaction ID | Segment ID | Named System | Operation Name | Business Purpose | Candidate Component ID | Mock Needed? |
|---|---|---|---|---|---|---|
| SYS-001 | SEG-01 | ServiceNow | `checkEntitlement` | Confirm support tier and case context before triage | API-001 | Yes |

## Rationale

ServiceNow is used as the named support-system mock because entitlement and open-case context are credible inputs for ticket triage. The demo uses a deterministic passthrough payload instead of a live ServiceNow API call.
