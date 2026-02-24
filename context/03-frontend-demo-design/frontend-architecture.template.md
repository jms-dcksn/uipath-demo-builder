# Frontend Architecture Template

Reference stack: Vite + React + TypeScript + UiPath TypeScript SDK.

## 1) Experience Goal

- Demo narrative:
- Primary user persona:
- Top outcomes user should achieve:

## 2) Application Structure

- Shell/layout model (must be multi-page):
- Header requirement (must span full width):
- Sidebar requirement (must be collapsible):
- Sidebar menu placeholders relevant to this use case:
- Route map:
- Protected routes and auth assumptions:
- SDK services used (`@uipath/uipath-typescript`):

### Sidebar Menu Placeholder Set (Required)

| Menu Entry | Purpose | Functional In Demo |
|---|---|---|
| Dashboard | Primary workload view | Yes |
| Intake Queue | Demonstration-only navigation affordance | No (placeholder) |
| Exception Worklist | Demonstration-only navigation affordance | No (placeholder) |
| Reports | Demonstration-only navigation affordance | No (placeholder) |
| Settings | Demonstration-only navigation affordance | No (placeholder) |

### Required Route Map

| Route | Page | Purpose | Entry Trigger |
|---|---|---|---|
| `/dashboard` | Dashboard | Monitor and triage case/process workload | Default landing after auth |
| `/instances/:instanceId` | Instance detail | Show process/case instance details, variables, metadata, and actions | Click row in dashboard table |
| `/settings` | Placeholder | Non-functional menu entry used for demo realism | Sidebar menu click |
| `/reports` | Placeholder | Non-functional menu entry used for demo realism | Sidebar menu click |

## 3) Data Flow

| UI Layer | Data Source | Read/Write | Notes |
|---|---|---|---|
| Dashboard case/process table | ProcessInstances + CaseInstances APIs | Read | Unified table for case rows and process-instance rows |
| Instance detail overview | ProcessInstances + Cases APIs | Read | Metadata, status, owner, timestamps, orchestration context |
| Variable panels | Process instance variables + case variables | Read | Render key/value variables with type-aware fallback |
| Task action panel | Tasks APIs + Cases APIs | Write | Submits decision and updates case/process state |
| Case entity panel (placeholder) | Data Fabric Entities API | Read/Write | Enabled only after case entity is confirmed created |

## 4) State Management Approach

- Server state strategy:
- Client state strategy:
- Polling/refresh strategy:

## 5) Navigation Behavior

- Row click behavior from dashboard table (case or process instance):
- Detail page route parameter rules (`instanceId`, optional `caseId`):
- Back navigation behavior to preserve dashboard filters/pagination:

## 6) Placeholder Integration Methods (Data Fabric)

Use stubs now and replace implementations after the user confirms the Data Fabric entity exists.

```typescript
// src/services/caseEntityDataSource.ts
// Placeholder methods for future Data Fabric wiring.
// Replace throw/return stubs once entity creation is confirmed.

export async function getCaseEntityByCaseId(caseId: string): Promise<Record<string, unknown> | null> {
  // TODO: wire to new Entities(sdk).getAll(...) filter by caseId once entity exists.
  return null;
}

export async function updateCaseEntityByCaseId(
  caseId: string,
  patch: Record<string, unknown>
): Promise<void> {
  // TODO: wire to Entities upsert/update once entity schema is confirmed.
  throw new Error("Case entity not wired yet. Confirm Data Fabric entity creation first.");
}

export async function listDashboardInstances(): Promise<Array<Record<string, unknown>>> {
  // TODO: wire to ProcessInstances/Cases list APIs; keep paginated.
  return [];
}

export async function getInstanceDetail(instanceId: string): Promise<Record<string, unknown> | null> {
  // TODO: wire to ProcessInstances/Cases getById APIs.
  return null;
}
```

## 7) Error And Empty States

- Empty list state:
- API error state:
- Partial data state:

## 8) Performance Targets

- Initial load target:
- List interaction target:
- Detail page load target:

## 9) Accessibility And Usability

- Keyboard support requirements:
- Contrast/readability requirements:
- Form validation requirements:

## 10) Demo-First Engineering Notes

- Seed data strategy:
- Tenant/environment configuration assumptions:
- Mock/fallback plan for unavailable integrations:
