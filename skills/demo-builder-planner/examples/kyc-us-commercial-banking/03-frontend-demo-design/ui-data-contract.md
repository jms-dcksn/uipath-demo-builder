# UI Data Contract - KYC Demo App

## 1) Contract Scope

- Version: 2.0.0
- Pages covered (`/dashboard`, `/instances/:instanceId`, placeholders): Dashboard, Instance Detail, placeholder routes (`/intake`, `/exceptions`, `/reports`, `/settings`)
- Related entity schema version: `KycOnboardingCase` Data Fabric entity-definition v2
- Related process/case API versions: Current tenant ProcessInstances/CaseInstances APIs

## 2) Dashboard Contracts

| UI Component | Data Field/Source | Required | Transformation | Fallback |
|---|---|---|---|---|
| Header title bar (full width) | Static + tenant/org context | Yes | None | Static title only |
| Sidebar menu | Static placeholder entries | Yes | Use-case labels | Hide optional entries |
| Record type column | Derived from instance payload | Yes | Map to `Case`/`Process Instance` | `Unknown` |
| Instance ID column | processInstanceId or caseInstanceId | Yes | None | `-` |
| Case ID column | caseId | No | None | `Not linked` |
| Stage/state chip | stage/status | No | Map internal states to display label | `Unknown` |
| Active task chip | currentTaskId/taskName | No | Map task ID to task name | `No active task` |
| Row click navigation | selected row instanceId | Yes | Route to `/instances/:instanceId` | Block navigation + toast |

## 3) Instance Detail Contracts

| UI Component | Data Field/Source | Required | Transformation | Fallback |
|---|---|---|---|---|
| Instance metadata panel | Process/Cases getById response | Yes | Normalize keys for display | Show partial panel with missing indicators |
| Process/case variables table | Variables endpoints | No | Format by primitive/object/list type | `No variables returned` |
| Timeline panel | task/event API | No | Sort by timestamp desc | `No events` |
| Related case entity panel (placeholder) | Data Fabric entity lookup by `caseid` (mapped from `caseId`) | No | Merge into detail context | `Entity not connected` badge |

## 4) Write Contracts

| UI Action | Payload Field | Validation Rule | Destination |
|---|---|---|---|
| Submit decision | status | Must be Approved or Rejected | Case entity update |
| Submit decision | decisionreasoncode | Required for approvals/rejections | Task completion API |
| Submit decision | reviewercomment | Min length 10 chars | Task completion API |
| Request info | requestedFields[] | Must contain at least one field | Case + task update API |
| Reassign workload | assignedto, assignedteam | `assignedto` must map to valid user | Case/task assignment API |

## 5) Placeholder Integration Contracts (Data Fabric)

| Method | Input | Expected Output | Current Behavior | Enablement Trigger |
|---|---|---|---|---|
| `getCaseEntityByCaseId(caseId)` | `caseId: string` | case entity record or `null` | Returns `null` | User confirms entity created |
| `updateCaseEntityByCaseId(caseId, patch)` | case ID + patch object | success/failure | Throws not wired error | User confirms entity created |
| `enrichRowWithCaseEntityData(caseId)` | `caseId: string` | merged row model | No-op passthrough | Entity read available |

## 6) Error Contracts

| Condition | UI Behavior | Recovery Path |
|---|---|---|
| Missing required field | Block submit and show inline error | User corrects and resubmits |
| Missing instance metadata | Show partial detail and warning banner | Retry load; allow back navigation |
| Entity update conflict | Show stale-data warning | Reload latest case and reapply decision |
| Data Fabric not wired | Show informational placeholder in related panel | Continue with process/case sources only |

## 7) Contract Tests

- Test case: Open mixed workload row and navigate to `/instances/:instanceId`.
- Expected result: Detail page renders metadata and variables with fallback handling.
- Error-to-UI mapping validated: Missing reason code blocks decision submission with inline guidance.
