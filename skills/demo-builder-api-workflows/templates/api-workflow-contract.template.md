# Mock API Workflow Contract

## 1) Identity

- API ID: `API-001`
- Workflow name:
- Named system:
- Operation name:
- Business purpose:
- Mock status: deterministic passthrough
- Artifact path: `apis/API-001/<WorkflowName>.json`

## 2) Source Notes

| Source ID | Source Type | Used For | Notes |
|---|---|---|---|
| SRC-001 | Public API docs / connector docs / sample payload / user doc | Payload convention |  |

## 3) Response Payload Summary

| JSON Path | Example Value | Field Class | Downstream Consumer | Demo Visible? |
|---|---|---|---|---|
| `$.account.accountId` | `ACC-100245` | Identity | AG-001 | Yes |
| `$.entitlement.status` | `active` | Decision | CTRL-001 | Yes |
| `$.entitlement.summary` | `Premium support active` | Narrative | AG-001 | Yes |

## 4) Output Schema

- Schema source: generated from `payload.json`.
- Response node source: same `payload.json` content.
- Drift check:

## 5) Flow Binding

| Flow Field | Value |
|---|---|
| Planned Flow node ID |  |
| Registry node type | `uipath.core.api-workflow.<resourceKey>` |
| Node category | `api-workflow` |
| Model service type | `Orchestrator.ExecuteApiWorkflowAsync` |
| Binding resource subtype | `Api` |
| Binding orchestrator type | `api` |
| Output variable | `$vars.<apiNodeId>.output` |
| Fallback if not bindable | Fixture-backed Flow tool injection |

## 6) Validation

| Check | Command or Review | Result | Notes |
|---|---|---|---|
| Payload JSON valid | Structured parser | Pending |  |
| Field map paths exist | JSON path review | Pending |  |
| Workflow JSON valid | Structured parser | Pending |  |
| Local API workflow run | `uip api-workflow run <Workflow.json> --no-auth` | Pending |  |
| Project build | `uip api-workflow build <projectPath>` | Not applicable | Only for project-backed workflows |
| Project pack | `uip api-workflow pack <projectPath> <destinationPath>` | Not applicable | Only for project-backed workflows |

## 7) Safety Review

- No real customer data:
- No real patient data:
- No real account numbers:
- No credentials or live endpoints:
