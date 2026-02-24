# Acceptance Scenarios Template

## 1) Scope

- Orchestration model: `BPMN | Case Management`
- In-scope segments:
- In-scope task IDs:

## 2) Scenario Catalog

| Scenario ID | Path Type | Segment(s) | Trigger | Expected Outcome |
|---|---|---|---|---|
| SCN-001 | Happy path | SEG-01 -> SEG-04 | Valid submission | Final decision recorded and visible in UI |
| SCN-002 | Exception path | SEG-01 | Low extraction confidence | Manual validation task created |
| SCN-003 | Demo script validation | Cross-segment | Script walkthrough rehearsal | Key messages and visual proofs align with actual build |

## 3) Detailed Scenario Template

### Scenario: `<Scenario ID>`

- Preconditions:
- Steps:
- Expected orchestration behavior:
- Expected data updates:
- Expected UI behavior:
- Expected audit events:

## 4) Signoff

- Signoff criteria:
- Reviewer:
- Date:

## 5) Demo Script Validation Checklist

- Script contains `3-4` key messages.
- Each key message maps to `2-3` visuals.
- Every visual beat has narration + operator action + visible outcome.
- Closing statement ties to business impact (speed, risk, quality, or cost).
