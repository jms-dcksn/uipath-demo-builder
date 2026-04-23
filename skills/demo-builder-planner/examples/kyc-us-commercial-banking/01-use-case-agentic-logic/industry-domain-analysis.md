# Industry And Domain Analysis - KYC Onboarding

## 1) Domain Boundary

- Primary industry: Banking
- Sub-domain: KYC onboarding for business customers
- Business capability in focus: Pre-account-opening compliance checks
- Neighboring capabilities: Credit underwriting, fraud monitoring, periodic KYC refresh
- Back-office operation in focus: Analyst and compliance adjudication workflow

## 2) Persona And Role Model

| Role | Goal | Decisions They Make | System Touchpoints |
|---|---|---|---|
| KYC Analyst | Complete onboarding case quickly and correctly | Approve/request info/reject recommendation | Dashboard, instance detail, task queue |
| Compliance Reviewer | Resolve high-risk or hit cases | Escalate, reject, approve with conditions | Instance detail evidence and decision panel |
| Operations Admin | Keep flow healthy | Reassign tasks and resolve blocked cases | Queue management dashboard |

## 3) Event Taxonomy

| Event | Source | Meaning | Downstream Action |
|---|---|---|---|
| CaseSubmitted | Frontend | New KYC onboarding request created | Start SEG-01 |
| DocumentsExtracted | IDP | Document fields parsed | Trigger T-003 validation |
| ScreeningHitDetected | Screening API | Potential sanctions/watchlist match | Create adjudication task |
| DecisionSubmitted | Human Task | Reviewer provided final decision | Trigger SEG-04 completion tasks |

## 4) Regulatory/Policy Constraints

- Data handling constraints: PII-sensitive customer/owner identity data must be access-controlled.
- Audit requirements: Preserve evidence trail and decision rationale for each case.
- SLA rules: Standard review in 8 business hours; escalated cases in 24 hours.
- Segregation of duties: High-risk cases require compliance reviewer approval.

References: [SRC-001], [SRC-003], [SRC-004], [SRC-005], [SRC-006]

## 5) Decision Inventory

| Decision Point | Inputs | Rule Type | Decision Owner |
|---|---|---|---|
| Completeness pass/fail | Required field set + confidence | Deterministic | KYC policy owner |
| Ownership compliance pass/fail | Beneficial owner structure | Deterministic + assisted | Compliance |
| Screening hit disposition | Hit metadata + evidence context | Human judgment | Compliance reviewer |
| Final approval/rejection | Aggregate risk + policy criteria | Human accountable decision | KYC analyst/reviewer |

## 6) Process Variants

- Standard path: Complete docs, no screening hits, low risk -> fast adjudication.
- Fast-track path: Returning known customer with low incremental risk.
- Exception path: Missing data, conflicting ownership, or screening hit.
- Human override path: Reviewer manually overrides agent recommendation with rationale.

## 7) Domain Glossary

| Term | Definition | Synonyms To Avoid |
|---|---|---|
| KYC Case | End-to-end onboarding record for a customer relationship decision | Ticket |
| Beneficial Owner | Individual who meets policy ownership/control threshold | Shareholder (generic) |
| Screening Hit | Potential match requiring analysis, not automatic reject | Positive match |
