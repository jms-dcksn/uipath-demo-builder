# Dashboard Page Template

## 1) Page Objective

- What user should accomplish:
- Why this page matters in the demo:

## 2) Core Layout

- Header content (full-width, fixed or sticky):
- Header actions (refresh, sign-out, environment badge):
- Sidebar behavior (collapsible):
- Sidebar menu placeholders (use-case relevant, may be non-functional):
- KPI strip/cards:
- Filters/search area:
- Case/process instance list area:

### Sidebar Placeholder Menu Entries

| Entry | Target Route | Notes |
|---|---|---|
| Dashboard | `/dashboard` | Active item by default |
| Intake Queue | `/intake` | Placeholder route/page allowed |
| Exception Worklist | `/exceptions` | Placeholder route/page allowed |
| Reports | `/reports` | Placeholder route/page allowed |
| Settings | `/settings` | Placeholder route/page allowed |

## 3) Unified List Specification (Cases + Process Instances)

| Column | Source Field | Sortable | Filterable | Notes |
|---|---|---|---|---|
| Record Type | Derived (`Case` or `Process Instance`) | Yes | Yes | Allows mixed workload view |
| Instance ID | processInstanceId or caseInstanceId | Yes | Yes | Click opens detail page |
| Case ID | caseId | Yes | Yes | Optional if not linked |
| Current Stage/State | stage/status | Yes | Yes | Shows current workflow location |
| Active Task | currentTaskId/taskName | No | Yes | Indicates next work item |
| Automation Type | Derived from task map | No | Yes | AI Agent/RPA/API/IDP/Human |
| Updated At | updateTime/lastModified | Yes | Yes | ISO-8601 to local display |

## 4) Actions

| Action | Trigger | Behavior | Backend Effect |
|---|---|---|---|
| Open instance detail | Row click on case or process instance | Navigate to `/instances/:instanceId` | None |
| Refresh dashboard rows | Refresh button | Reload list and counters | Re-query process/case/entity/task endpoints |
| Toggle sidebar | Sidebar toggle button | Collapse/expand sidebar menu labels | None |

## 5) States

- Loading behavior:
- Empty results behavior:
- Error behavior:
- Partial data behavior when variables/metadata are missing:

## 6) Demo Moments

- Highlightable metric:
- Story beat when filter is used:
- Story beat when a high-risk case appears:
- Script visual ID(s) for this page (for example `VIS-01`, `VIS-02`):

## 7) Placeholder Methods For Data Fabric

Document where Data Fabric case entity calls will be inserted after entity creation confirmation.

- `loadDashboardRows()`:
- `enrichRowWithCaseEntityData(caseId)`:
- `refreshRowFromCaseEntity(caseId)`:
