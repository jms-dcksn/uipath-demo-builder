# UI Data Contract Template

## 1) Contract Scope

- Version:
- Pages covered (`/dashboard`, `/instances/:instanceId`, placeholders):
- Related entity schema version:
- Related process/case API versions:

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
| Process/case variables table | Variables endpoints | No | Format value by primitive/object/list | `No variables returned` |
| Timeline panel | Audit/history source | No | Sort descending by timestamp | Empty state message |
| Related case entity panel (placeholder) | Data Fabric entity lookup by caseId | No | Merge into detail context | `Entity not connected` badge |

## 4) Write Contracts

| UI Action | Payload Field | Validation Rule | Destination |
|---|---|---|---|
| Submit decision | status | Must be Approved or Rejected | Case entity update |
| Submit decision | reasonCode | Required for approval/rejection | Task completion API |
| Request info | requestedFields[] | Must contain at least one field | Case + task update API |

## 5) Placeholder Integration Contracts (Data Fabric)

| Method | Input | Expected Output | Current Behavior | Enablement Trigger |
|---|---|---|---|---|
| `getCaseEntityByCaseId(caseId)` | `caseId: string` | case entity record or `null` | Returns `null` | User confirms entity created |
| `updateCaseEntityByCaseId(caseId, patch)` | case ID + patch object | success/failure | Throws not wired error | User confirms entity created |
| `enrichRowWithCaseEntityData(caseId)` | `caseId: string` | merged row model | No-op passthrough | Entity read available |

## 6) Error Contracts

| Condition | UI Behavior | Recovery Path |
|---|---|---|
| Missing required field | Block submit and show inline error | User corrects input |
| Missing instance metadata | Show partial detail and warning banner | Retry load; allow back navigation |
| Data Fabric not wired | Show informational placeholder in related panel | Continue with process/case sources only |

## 7) Contract Tests

- Test case:
- Expected result:
- Error-to-UI mapping validated:
