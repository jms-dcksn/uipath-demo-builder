# Agent Build Spec - AG-001 Ticket Triage

## Agent Index

| Agent ID | Agent Name | Mode | Project Path / Registry Name | Flow Node ID | Inputs | Outputs |
|---|---|---|---|---|---|---|
| AG-001 | `ticket-triage-agent` | coded | `agents/AG-001/ticket-triage-agent` | classifyUrgency | normalized ticket | urgency JSON |

## Flow Contract

| Flow Mapping | Value |
|---|---|
| Agent node output source | `$vars.classifyUrgency.output.content` |
| Parsed output shape | `{ "urgency": "normal|high", "reason": "...", "nextAction": "notify|review" }` |
| Downstream node | CTRL-001 |

## Validation

- Happy path: billing question, normal urgency, route to notify.
- Exception path: production outage, high urgency, route to review.
