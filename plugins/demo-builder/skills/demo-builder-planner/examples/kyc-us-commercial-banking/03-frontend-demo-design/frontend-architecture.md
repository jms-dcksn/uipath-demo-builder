# Frontend Architecture - KYC Demo App

Reference stack: Vite + React + TypeScript + UiPath TypeScript SDK.

## 1) Experience Goal

- Demo narrative: Show operations users how KYC workload is triaged on a dashboard, then resolved through an instance detail workflow.
- Primary user persona: KYC Analyst.
- Top outcomes user should achieve:
  - Prioritize high-risk and exception work quickly.
  - Open a case/process instance from the queue and complete adjudication.
  - Review metadata, variables, and evidence before decision submission.

## 2) Application Structure

- Shell/layout model (must be multi-page): Shared app shell with full-width header, collapsible left sidebar, route-driven main content.
- Header requirement (must span full width): Header spans viewport width, displays environment badge, refresh, and sign-out.
- Sidebar requirement (must be collapsible): Icon-only collapsed state plus expanded labeled state.
- Sidebar menu placeholders relevant to this use case: Include non-functional placeholders to make the demo feel production-like.
- Route map:
  - `/dashboard`
  - `/instances/:instanceId`
  - `/intake` (placeholder)
  - `/exceptions` (placeholder)
  - `/reports` (placeholder)
  - `/settings` (placeholder)
- Protected routes and auth assumptions: Authenticated enterprise users only.
- SDK services used (`@uipath/uipath-typescript`): `cases`, `maestro-processes`, `tasks`, `entities` (placeholder integration after entity confirmation).

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
| Variable panels | Process instance variables + case variables | Read | Render key/value variables with typed fallback |
| Task action panel | Tasks APIs + Cases APIs | Write | Submits decision and updates case/process state |
| Case entity panel (placeholder) | Data Fabric Entities API | Read/Write | Enabled only after entity creation is confirmed |

## 4) State Management Approach

- Server state strategy: React Query with paginated fetches and scoped invalidation by route.
- Client state strategy: Route-local UI state for filters, table density, and staged decision form values.
- Polling/refresh strategy: 30-second dashboard refresh plus explicit manual refresh in header.

## 5) Navigation Behavior

- Row click behavior from dashboard table (case or process instance): Navigate to `/instances/:instanceId`.
- Detail page route parameter rules (`instanceId`, optional `caseId`): `instanceId` required; `caseId` resolved from instance metadata and mapped to Data Fabric field `caseid` when querying entities.
- Back navigation behavior to preserve dashboard filters/pagination: Persist query params in URL and restore table state on back.

## 6) Placeholder Integration Methods (Data Fabric)

Use stubs now and replace implementations after the user confirms the Data Fabric entity exists.

```typescript
// src/services/caseEntityDataSource.ts
// Placeholder methods for future Data Fabric wiring.
// Replace throw/return stubs once entity creation is confirmed.

export async function getCaseEntityByCaseId(caseId: string): Promise<Record<string, unknown> | null> {
  // TODO: wire to new Entities(sdk).getAll(...) filter by Data Fabric field `caseid`.
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

- Empty list state: Show zero-state panel with filter reset and mock-seed CTA.
- API error state: Non-blocking banner with retry and correlation ID.
- Partial data state: Render available metadata/variables and flag missing source sections.

## 8) Performance Targets

- Initial load target: < 2.5 sec on demo environment.
- List interaction target: < 300 ms for filter/sort response.
- Detail page load target: < 1.5 sec for metadata + variable tabs.

## 9) Accessibility And Usability

- Keyboard support requirements: Full table navigation and action controls accessible by keyboard.
- Contrast/readability requirements: WCAG AA contrast and focus indicators.
- Form validation requirements: Inline validation with actionable correction messages.

## 10) Demo-First Engineering Notes

- Seed data strategy: Pre-seed mixed normal, high-risk, and exception instances.
- Tenant/environment configuration assumptions: OAuth app and required scopes configured.
- Mock/fallback plan for unavailable integrations: Route adapters to mock providers while preserving payload contracts.
