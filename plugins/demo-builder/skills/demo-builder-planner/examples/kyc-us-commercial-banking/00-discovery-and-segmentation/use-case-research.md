# Use Case Research - KYC Onboarding (US Commercial Banking)

## 1) Research Brief

- Use case: New business account onboarding with KYC controls
- Industry: Banking
- Domain: BSA/AML onboarding operations
- Research objective: Define a realistic KYC operating model that can be demoed with automation + human review
- Date: 2026-02-21

## 2) Search Strategy

- Primary questions:
  - What minimum KYC/CIP/CDD steps are expected for US banks?
  - Where are human decisions mandatory vs automatable?
  - What evidence must be retained for audit?
- Keywords and variants:
  - "bank customer due diligence requirements"
  - "31 CFR 1020.220 CIP"
  - "31 CFR 1010.230 beneficial ownership"
  - "OFAC sanctions screening compliance framework"
- Inclusion criteria:
  - Regulator or official standard-body sources
  - Current guidance pages and legal text references
- Exclusion criteria:
  - Vendor marketing without regulatory references
  - Unattributed opinion content

## 3) Source Log

| Source ID | URL | Type | Why Trusted | Key Findings | Relevance |
|---|---|---|---|---|---|
| SRC-001 | https://www.fincen.gov/resources/statutes-regulations/federal-register-notices/customer-due-diligence-requirements | US regulator page | FinCEN source | CDD rule baseline and beneficial ownership expectations | High |
| SRC-002 | https://www.law.cornell.edu/cfr/text/31/1010.230 | Legal text reference | CFR mirror and citation-friendly | Beneficial ownership data collection expectations for legal entities | High |
| SRC-003 | https://www.law.cornell.edu/cfr/text/31/1020.220 | Legal text reference | CFR mirror and citation-friendly | CIP controls for customer identification and recordkeeping | High |
| SRC-004 | https://bsaaml.ffiec.gov/manual/AssessingComplianceWithBSARegulatoryRequirements/02 | US supervisory manual | FFIEC interagency exam manual | CDD program expectations and risk-based customer profiles | High |
| SRC-005 | https://home.treasury.gov/news/press-releases/sm680 | Treasury guidance | Official OFAC publication | Core compliance framework elements for sanctions controls | Medium |
| SRC-006 | https://ofac.treasury.gov/sanctions-list-search-tool | Treasury operational tool | Official sanctions source | Required screening touchpoint in onboarding flow | High |
| SRC-007 | https://www.fatf-gafi.org/en/publications/Fatfrecommendations/Risk-based-approach-banking-sector.html | Global standard guidance | FATF standards body | Risk-based approach supports segmented risk workflows and review depth | Medium |

## 4) Operating Model Summary

- Trigger events: Customer submits new business account application and required identity/formation documents.
- Inputs: Application form, business registration docs, beneficial ownership details, identity documents.
- Core activities:
  - Capture + extract onboarding package
  - Validate completeness and identity data
  - Screen sanctions/PEP/adverse risk indicators
  - Perform risk scoring and reviewer adjudication
  - Final decision and downstream account setup
- Decision points:
  - Is required KYC evidence complete?
  - Do screening results require escalation?
  - Is risk score within auto-approval tolerance?
- Output/outcomes: Approved onboarding case, rejected case, or pending information request.

## 5) Common Process Patterns

- Pattern 1: Intake -> data extraction -> data validation -> screening -> adjudication.
- Pattern 2: Risk-based depth (simplified review for low risk, enhanced review for higher risk).
- Pattern 3: Mandatory audit trail of checks, evidence, and reviewer rationale.

## 6) Exception Patterns

- Exception type: Missing beneficial ownership fields
  - Typical cause: Incomplete applicant submission
  - Operational impact: Case stalls and breaches SLA
  - Typical mitigation: Generate follow-up task for applicant/operations
- Exception type: Screening hit (name similarity)
  - Typical cause: Potential sanctions or watchlist similarity
  - Operational impact: Manual disposition required
  - Typical mitigation: Human review with evidence and escalation policy
- Exception type: External API outage
  - Typical cause: Third-party dependency unavailability
  - Operational impact: Delayed review
  - Typical mitigation: Retry policy + fallback to manual or RPA-assisted lookup

## 7) Assumptions And Unknowns

- Assumption: Demo tenant supports required API connectors and task workflows.
- Assumption: KYC policy thresholds are provided by compliance SMEs.
- Unknown: Which external screening provider will be used in the demo.
- Owner/date for resolution: Demo architect, 2026-02-24.

## 8) Recommended Demo Slice

- Why this slice is demo-worthy: It shows AI-assisted automation plus accountable human decisioning with clear business impact.
- Boundaries: Business checking onboarding only; exclude lending products and post-onboarding monitoring.
- Success metric: Reduce median case resolution time from 2 business days to under 6 hours for clean low-risk cases.
