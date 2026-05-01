# Payload Field Map - Support Ticket Flow

| API ID | JSON Path | Example Value | Field Class | Downstream Consumer | Consumer Type | Demo Script Visible? |
|---|---|---|---|---|---|---|
| API-001 | `$.account.accountId` | `ACC-100245` | Identity | AG-001 / FLOW-END-01 | agent-input / end-output | Yes |
| API-001 | `$.account.customerTier` | `premium` | Decision | AG-001 | agent-input | Yes |
| API-001 | `$.entitlement.status` | `active` | Decision | CTRL-001 | condition-input | Yes |
| API-001 | `$.entitlement.summary` | `Premium support entitlement is active for the reported product.` | Narrative | AG-001 / CONN-001 | agent-input / connector-input | Yes |
| API-001 | `$.caseContext.recommendedRoute` | `priority-support` | Decision | FLOW-END-01 | end-output | Yes |
