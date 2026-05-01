# Agent Build Spec - AG-001 Ticket Triage

## Agent Index

| Agent ID | Agent Name | Mode | Project Path / Registry Name | Flow Node ID | Inputs | Outputs |
|---|---|---|---|---|---|---|
| AG-001 | `ticket-triage-agent` | coded | `agents/AG-001/ticket-triage-agent` | classifyUrgency | normalized ticket + documented API-001 entitlement fields | urgency JSON |

## Flow Contract

| Flow Mapping | Value |
|---|---|
| Agent input source | `=js:{ ticket: $vars.normalizeTicket.output, entitlement: $vars.serviceNowEntitlementLookup.output.entitlement, account: $vars.serviceNowEntitlementLookup.output.account }` |
| Agent node output source | `$vars.classifyUrgency.output.content` |
| Parsed output shape | `{ "urgency": "normal|high", "reason": "...", "nextAction": "notify|review" }` |
| Downstream node | CTRL-001 |

## Validation

- Happy path: billing question, normal urgency, route to notify.
- Exception path: production outage, high urgency or inactive entitlement, route to review.
