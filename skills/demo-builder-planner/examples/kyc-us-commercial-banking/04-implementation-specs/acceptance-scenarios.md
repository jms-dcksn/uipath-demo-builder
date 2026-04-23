# Acceptance Scenarios - KYC Onboarding Demo

## 1) Scope

- Orchestration model: `Case Management`
- In-scope segments: SEG-01, SEG-02, SEG-03, SEG-04
- In-scope task IDs: T-001 through T-011

## 2) Scenario Catalog

| Scenario ID | Path Type | Segment(s) | Trigger | Expected Outcome |
|---|---|---|---|---|
| SCN-001 | Happy path | SEG-01 -> SEG-04 | Complete low-risk application | Case approved and closed with notifications |
| SCN-002 | Exception path | SEG-01, SEG-02 | Missing beneficial owner DOB | Case moves to PendingInfo with follow-up task |
| SCN-003 | Exception path | SEG-03 | Potential sanctions match | Manual adjudication required and recorded |
| SCN-004 | Failure path | SEG-04 | Core update RPA failure | Exception task created and case remains InReview |

## 3) Detailed Scenario Template

### Scenario: `SCN-001`

- Preconditions:
  - Valid application and required docs submitted.
  - Screening returns no hits.
- Steps:
  1. Execute T-001 to T-007.
  2. Reviewer opens case and submits approval in T-008.
  3. T-009/T-010/T-011 complete.
- Expected orchestration behavior: Case advances stage-by-stage without exception branches.
- Expected data updates: `status` transitions `New -> InReview -> Approved -> Closed`.
- Expected UI behavior: Dashboard row moves from open queue to closed state.
- Expected audit events: Full timeline with reviewer decision event and closure timestamp.

### Scenario: `SCN-003`

- Preconditions:
  - Screening API returns a potential hit with medium confidence.
- Steps:
  1. T-006 returns `PotentialMatch`.
  2. T-007 generates reviewer briefing.
  3. Reviewer adjudicates via T-008 and sets decision to `PendingInfo`.
- Expected orchestration behavior: Case remains active and routes to follow-up tasks.
- Expected data updates: `screeningstatus=PotentialMatch`, `status=PendingInfo`, `decisionreasoncode` captured.
- Expected UI behavior: Case appears in PendingInfo queue with escalation badge.
- Expected audit events: Hit adjudication reasoning recorded with reviewer identity.

## 4) Signoff

- Signoff criteria: All critical scenarios pass with expected data and UI behaviors.
- Reviewer: Demo architect + compliance SME.
- Date: 2026-03-10 (target).
