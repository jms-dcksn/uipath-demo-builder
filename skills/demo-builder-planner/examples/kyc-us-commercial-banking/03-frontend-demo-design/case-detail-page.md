# Instance Detail Page Specification - KYC Review

## 1) Page Objective

- Decision or action expected from user: Approve, reject, or request additional information with full instance context.
- Business value of this step: Converts automation outputs and case/process metadata into an accountable decision.

## 2) Core Sections

- Instance summary header (Case or Process Instance): Primary identifiers, status, risk tier, assignee, SLA timer.
- Instance metadata panel (IDs, status, owner, created/updated times, process definition): Platform metadata for traceability.
- Process/case variables panel (typed key/value rendering): Human-readable variables with JSON fallback rendering.
- Timeline/audit history: Ordered task outcomes and decision events.
- Evidence/documents panel: Document previews and extraction confidence context.
- Task execution summary panel (AI/RPA/API/IDP/Human): Per-task outcomes and exception visibility.
- Task review and decision panel: Decision controls, reason codes, and reviewer comments.
- Related case entity panel (placeholder until Data Fabric entity wiring is enabled): Contextual entity values once confirmed.

## 3) Task Review Flow

| Step | UI Element | Validation | Result |
|---|---|---|---|
| Review automated summary | Risk briefing card | Must show risk score, hit summary, and rationale | Reviewer context established |
| Inspect variables/metadata | Variables + metadata tabs | Must handle missing/partial values | Reviewer confirms current instance state |
| Validate evidence | Evidence checklist | Required docs/fields must be present | Decision controls enabled |
| Submit decision | Decision form | Reason code and comment required | Instance progresses to next orchestration path |

## 4) Decision Actions

| Action | Required Inputs | Entity Update | Orchestration Impact |
|---|---|---|---|
| Approve | reasonCode, comment | status=Approved, decision=Approved | Trigger finalization stage |
| Reject | reasonCode, comment | status=Rejected, decision=Rejected | Trigger rejection notification and closure |
| Request more info | comment, requestedFields | status=PendingInfo, decision=PendingInfo | Re-open intake/processing tasks |

## 5) Guardrails

- Required fields before submit: Decision reason code and reviewer comment.
- Permission checks: Only assigned reviewer or supervisor can submit decision.
- Conflict checks (stale data/version): Warn and reload if record version changed.
- Decision accountability checks (reason code/comment mandatory): Hard validation before submit.

## 6) States

- Loading state: Panel-level skeletons.
- Error state: Inline recovery for failed metadata/variable/evidence fetch.
- Completed/locked state: Read-only decision view after closure.

## 7) Detail Route + Context Rules

- Route shape for detail page (`/instances/:instanceId`): Required route parameter with optional resolved `caseId` from payload.
- How `instanceId` resolves to case/process APIs: Query process instance first, then linked case details/variables.
- Behavior when no linked case entity record exists yet: Render placeholder panel with non-blocking note.

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
