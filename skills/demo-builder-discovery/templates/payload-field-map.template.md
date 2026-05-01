# Payload Field Map

Map mock API payload fields to downstream Flow consumers.

## 1) Field Map

| API ID | JSON Path | Example Value | Field Class | Downstream Consumer | Consumer Type | Demo Script Visible? | Source / Rationale |
|---|---|---|---|---|---|---|---|
| API-001 | `$.account.accountId` | `ACC-100245` | Identity | AG-001 | agent-input | Yes | Trace account context |
| API-001 | `$.entitlement.status` | `active` | Decision | CTRL-001 | condition-input | Yes | Route entitlement exceptions |
| API-001 | `$.entitlement.summary` | `Premium support entitlement is active` | Narrative | AG-001 | agent-input | Yes | Agent reasoning context |
| API-001 | `$.caseContext.recommendedRoute` | `priority-support` | Decision | FLOW-END-01 | end-output | Yes | Final route output |

## 2) Consumer Summary

| Consumer ID | Required API Fields | Runtime Reference Pattern |
|---|---|---|
| AG-001 | `$.account.accountId`, `$.entitlement.summary` | `$vars.<apiNodeId>.output.account.accountId` |
| CTRL-001 | `$.entitlement.status` | `$vars.<apiNodeId>.output.entitlement.status` |

## 3) Validation Notes

- Payload file:
- Checked by:
- Missing paths:
- Fields intentionally excluded:
