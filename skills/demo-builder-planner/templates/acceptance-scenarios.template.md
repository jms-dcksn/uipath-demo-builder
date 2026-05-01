# Acceptance Scenarios Template

## 1) Scope

- Orchestration model: `Maestro Flow`
- In-scope segments:
- In-scope task IDs:
- Flow project:

## 2) Scenario Catalog

| Scenario ID | Path Type | Segment(s) | Trigger | Expected Outcome |
|---|---|---|---|---|
| SCN-001 | Happy path | SEG-01 -> SEG-03 | Valid trigger payload | End output returns approved demo result |
| SCN-002 | Exception path | SEG-01 -> SEG-02 | High-risk or incomplete payload | Flow routes to exception branch and returns a clear exception result |
| SCN-003 | Mock API path | System context segment | API Workflow project returns deterministic payload and is uploaded as `projectType: Api` | Agent and branch logic use documented payload fields through a native API node or script placeholder |
| SCN-004 | Demo script validation | Cross-segment | Script walkthrough rehearsal | Messages and proof points align with actual Flow |

## 3) Detailed Scenario Template

### Scenario: `<Scenario ID>`

- Preconditions:
- Manual-start input fields:
- Steps:
- Expected Flow behavior:
- Expected agent/connector/tool/control behavior:
- Expected mock API Workflow project behavior:
- Expected current Flow invocation mode:
- Expected End output:
- Expected audit or visible proof:

## 4) Signoff

- Signoff criteria:
- Reviewer:
- Date:

## 5) Demo Script Validation Checklist

- Script contains 3-4 key messages.
- Each key message maps to 2-3 proof points.
- Demo operator action names the visible trigger input fields used for the run.
- Mock API Workflow proof points distinguish uploaded API projects from native Flow nodes or script placeholders.
- Every beat has narration, operator action, and visible outcome.
- Closing statement ties to business impact.
