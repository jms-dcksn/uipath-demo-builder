# Dashboard Page Specification - KYC Queue

## 1) Page Objective

- What user should accomplish: Identify the highest-priority case/process instance and open the next work item quickly.
- Why this page matters in the demo: It proves operational control in the first 30 seconds.

## 2) Core Layout

- Header content (full-width, fixed or sticky): Queue title, tenant badge, refresh action, and last-sync timestamp.
- Header actions (refresh, sign-out, environment badge): Refresh, environment indicator, and account action menu.
- Sidebar behavior (collapsible): Toggle between expanded labels and icon-only navigation.
- Sidebar menu placeholders (use-case relevant, may be non-functional): Intake Queue, Exception Worklist, Reports, Settings.
- KPI strip/cards: Open workload, high-risk count, pending-info count, SLA breach risk.
- Filters/search area: Status, risk tier, assigned team, free-text search.
- Case/process instance list area: Unified table with row-click navigation.

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
| Record Type | Derived (`Case` or `Process Instance`) | Yes | Yes | Supports mixed workload view |
| Instance ID | processInstanceId or caseInstanceId | Yes | Yes | Click opens detail page |
| Case ID | caseId | Yes | Yes | Optional when no linked case |
| Current Stage/State | stage/status | Yes | Yes | Process position for triage |
| Active Task | currentTaskId/taskName | No | Yes | Current action point |
| Assigned To | assignedTo | Yes | Yes | Ownership and queue routing |
| Updated At | updateTime/lastModified | Yes | Yes | ISO-8601 converted to local time |
| Automation Type | Derived from task map | No | Yes | AI Agent / API / IDP / RPA / Human Task |

## 4) Actions

| Action | Trigger | Behavior | Backend Effect |
|---|---|---|---|
| Open instance detail | Row click on case or process instance | Navigate to `/instances/:instanceId` | None |
| Refresh dashboard rows | Refresh button | Reload list and counters | Re-query process/case/entity/task endpoints |
| Toggle sidebar | Sidebar toggle button | Collapse/expand sidebar labels | None |
| Reassign workload | Row action menu | Select new assignee/team | Update assignment fields |

## 5) States

- Loading behavior: Skeleton rows and KPI shimmer placeholders.
- Empty results behavior: Empty state panel with filter reset.
- Error behavior: Banner with retry and fallback to last successful snapshot.
- Partial data behavior when variables/metadata are missing: Keep row visible with missing-data chip.

## 6) Demo Moments

- Highlightable metric: High-risk workload decreases after reviewer actions.
- Story beat when filter is used: Filter to screening-hit workload to show exception handling.
- Story beat when a high-risk case appears: Open row and transition into instance detail decision flow.

## 7) Placeholder Methods For Data Fabric

Document where Data Fabric case entity calls will be inserted after entity creation confirmation.

- `loadDashboardRows()`: Aggregates case/process rows from APIs.
- `enrichRowWithCaseEntityData(caseId)`: No-op until entity integration is enabled.
- `refreshRowFromCaseEntity(caseId)`: Triggers row-level refresh once Data Fabric is wired.
