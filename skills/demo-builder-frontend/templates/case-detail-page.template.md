# Instance Detail Page Template

## 1) Page Objective

- Decision or action expected from user:
- Business value of this step:

## 2) Core Sections

- Instance summary header (Case or Process Instance):
- Instance metadata panel (IDs, status, owner, created/updated times, process definition):
- Process/case variables panel (typed key/value rendering):
- Timeline/audit history:
- Evidence/documents panel:
- Task execution summary panel (AI/RPA/API/IDP/Human):
- Task review and decision panel:
- Related case entity panel (placeholder until Data Fabric entity wiring is enabled):

## 3) Task Review Flow

| Step | UI Element | Validation | Result |
|---|---|---|---|
| Review recommendation | Recommendation card | Must render risk and rationale | User sees decision context |
| Inspect variables/metadata | Variables + metadata tabs | Must handle missing/partial values | User confirms instance context before decision |

## 4) Decision Actions

| Action | Required Inputs | Entity Update | Orchestration Impact |
|---|---|---|---|
| Approve | Comment, reason code | status=Approved | Move case to closure path |
| Request more info | Comment, required fields | status=PendingInfo | Re-open intake/validation tasks |

## 5) Guardrails

- Required fields before submit:
- Permission checks:
- Conflict checks (stale data/version):
- Decision accountability checks (reason code/comment mandatory):

## 6) States

- Loading state:
- Error state:
- Completed/locked state:
- Script visual ID(s) for this page (for example `VIS-03`, `VIS-04`):

## 7) Detail Route + Context Rules

- Route shape for detail page (`/instances/:instanceId`):
- How `instanceId` resolves to case/process APIs:
- Behavior when no linked case entity record exists yet:

## 8) Placeholder Methods For Future Data Fabric Wiring

```typescript
// src/services/instanceDetailDataSource.ts
export async function loadInstanceMetadata(instanceId: string): Promise<Record<string, unknown> | null> {
  // TODO: wire to ProcessInstances/Cases getById APIs.
  return null;
}

export async function loadInstanceVariables(instanceId: string): Promise<Record<string, unknown>> {
  // TODO: wire to process/case variable endpoints.
  return {};
}

export async function loadLinkedCaseEntity(caseId: string): Promise<Record<string, unknown> | null> {
  // TODO: wire to Data Fabric Entities API after entity creation is confirmed.
  return null;
}
```
