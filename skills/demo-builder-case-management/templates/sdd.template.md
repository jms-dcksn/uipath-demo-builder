# Case Management SDD

This minimal SDD is the handoff to the installed `uipath-case-management` skill. Keep it explicit and do not fabricate task type IDs, connection IDs, or resource IDs.

## 1) Case Definition

- Case name:
- Business objective:
- Case owner role:
- Case entity:
- Trigger component: `CMP-TRG-01`

## 2) Trigger

| Field | Value |
|---|---|
| Type |  |
| Display name |  |
| Input payload schema |  |
| Happy path fixture |  |
| Exception path fixture |  |

## 3) Stages

| Stage ID | Stage Name | Entry Criteria | Exit Criteria |
|---|---|---|---|
| STG-01 | Intake |  |  |

## 4) Tasks

| Stage | Task ID | Display Name | Component/Owner | Execution Type | Inputs | Outputs | Notes |
|---|---|---|---|---|---|---|---|
| Intake | T-001 |  | CMP-IDP-01 | IDP |  |  | Skeleton task if unresolved |
| Intake | T-002 |  | AG-001 | AI Agent |  |  | Agent task |
| Intake | T-003 |  | role:reviewer | Human Task |  |  | Action task |

## 5) Stub Contracts

Include every trigger, RPA, API, IDP, and intermediate event declared in the Case Management design.

| Component ID | Backs Task | Type | Inputs | Outputs | Demo Behavior | Real-Build Note |
|---|---|---|---|---|---|---|
| CMP-TRG-01 | Case start | Trigger |  |  |  |  |

## 6) Transitions And Conditions

| From | To | Condition | Rule ID |
|---|---|---|---|
| Trigger | Intake | Case created | R-001 |

## 7) SLA And Events

| ID | Scope | Type | Rule/Condition | Demo Timing | Notes |
|---|---|---|---|---|---|
| CMP-EVT-01 | Stage/task | Timer/message/signal |  |  |  |

## 8) Open Resource Placeholders

| Placeholder | Reason | Manual Completion Owner |
|---|---|---|
| `<UNRESOLVED: task-type-id>` | Real task resource not available locally |  |

