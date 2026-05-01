# Mock API Workflow Contract - API-001

## Identity

- API ID: `API-001`
- Workflow name: `ServiceNowEntitlementLookup`
- Named system: ServiceNow
- Operation name: `checkEntitlement`
- Business purpose: return deterministic support entitlement context for ticket triage.
- Mock status: deterministic passthrough.
- API project directory: `flow/<SolutionName>/ServiceNowEntitlementLookupApi/`.
- Upload `projectType`: `Api`.

## Response Payload Summary

| JSON Path | Example Value | Field Class | Downstream Consumer |
|---|---|---|---|
| `$.account.accountId` | `ACC-100245` | Identity | AG-001 / FLOW-END-01 |
| `$.account.customerTier` | `premium` | Decision | AG-001 |
| `$.entitlement.status` | `active` | Decision | CTRL-001 |
| `$.entitlement.summary` | `Premium support entitlement is active for the reported product.` | Narrative | AG-001 / CONN-001 |
| `$.caseContext.recommendedRoute` | `priority-support` | Decision | FLOW-END-01 |

## Flow Invocation

| Flow Field | Value |
|---|---|
| Planned Flow node ID | serviceNowEntitlementLookup |
| Registry node type | `uipath.core.api-workflow.<servicenow-entitlement-resource>` |
| Node category | `api-workflow` |
| Model service type | `Orchestrator.ExecuteApiWorkflowAsync` |
| Binding resource subtype | `Api` |
| Binding orchestrator type | `api` |
| Output variable when natively bound | `$vars.serviceNowEntitlementLookup.output` |
| Current invocation mode | `script placeholder` until native API Workflow node binding is available |

## Validation

| Check | Result |
|---|---|
| Payload JSON valid | Example artifact only |
| Field map paths exist | Example artifact only |
| Local API workflow run | Pending in real build |
| Project build | Pending in real build |
| Solution registration | Pending in real build |
| Upload confirmation | Pending in real build |
