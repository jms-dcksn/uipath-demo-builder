# Use Case Brief - KYC Onboarding

## 1) Use Case Metadata

- Use case name: `CommercialBankingKycOnboarding`
- Version: 1.0.0
- Author: Demo Builder Team
- Date: 2026-02-21
- Industry: Banking
- Domain: BSA/AML KYC onboarding
- Discovery source packet: `../00-discovery-and-segmentation/`

## 2) Problem Statement

- Current pain/problem: Onboarding is slow, manual, and inconsistent across reviewers.
- Why now: Business unit wants faster account opening without weakening controls.
- Business impact of current state: Lost customer conversions, SLA breaches, and audit remediation effort.

## 3) Target Outcome

- Desired end state: Automated pre-review and risk triage with human-in-the-loop final decisions.
- Business KPIs to improve: cycle time, first-pass completeness, review effort per case.
- Demo success criteria: Show clear straight-through low-risk path and controlled exception handling path.

## 4) Actors And Stakeholders

- End users: KYC analysts, compliance reviewers, onboarding operations.
- Process owners: Head of onboarding operations.
- Compliance/risk stakeholders: AML compliance and financial crime risk.
- Systems involved: Data Fabric, Maestro/Case Mgmt, sanctions screening APIs, legacy core banking system.

## 5) Scope Definition

- In scope: New US business checking onboarding KYC checks and decisioning.
- Out of scope: Transaction monitoring, ongoing periodic refresh, lending products.
- Phase 2 candidates: Continuous monitoring and document refresh campaigns.

## 6) User Journeys

| Journey | Trigger | Core Steps | Expected Outcome |
|---|---|---|---|
| Clean low-risk onboarding | Complete application | Intake -> automated checks -> quick human review -> approve | Approved within SLA |
| Exception onboarding | Missing fields or hit detected | Intake -> validation failure or screening hit -> investigation -> decision | Pending info or rejected with rationale |

## 7) Logical Segments

| Segment ID | Segment Name | Why It Exists | Entry Trigger | Exit Outcome |
|---|---|---|---|---|
| SEG-01 | Document Intake | Normalize inbound documents and form data | Application submitted | Structured intake package |
| SEG-02 | Application Processing | Validate and enrich customer profile | Intake package ready | Verified profile |
| SEG-03 | Eligibility Review | Assess compliance risk and determine disposition | Profile verified | Decision recommendation and action |
| SEG-04 | Final Decision | Finalize case and trigger downstream updates | Reviewer decision | Case closed with audit trail |

## 8) Technology Mix Summary

| Capability | Included? | Role In Demo |
|---|---|---|
| AI Agent | Yes | Completeness validation, ownership checks, reviewer briefing |
| RPA | Yes | Legacy core banking update fallback |
| IDP | Yes | Document data extraction |
| API Integration | Yes | Screening, registry lookup, notifications, case updates |
| Human Task | Yes | Final adjudication and exception resolution |

## 9) Constraints And Risks

- Operational constraints: Limited reviewer capacity during peak periods.
- Regulatory constraints: CIP/CDD evidence and sanctions screening traceability.
- Demo realism constraints: External providers may be mocked in non-prod.
- Top risks: Incomplete source documents, false-positive screening hits, API outages.

## 10) Assumptions

- Compliance SME provides risk thresholds and decision criteria.
- Demo tenant has task, entity, and integration capabilities enabled.

## 11) Open Questions

- Question: Which sanctions and adverse media provider will be used in final build?
- Owner: Integration lead
- Due date: 2026-02-24
