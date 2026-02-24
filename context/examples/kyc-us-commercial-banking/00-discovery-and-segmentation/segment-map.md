# Segment Map - KYC Onboarding

## 1) Segment Overview

| Segment ID | Segment Name | Business Goal | Entry Trigger | Exit Outcome |
|---|---|---|---|---|
| SEG-01 | Document Intake | Capture and normalize onboarding package | New application submitted | Documents and fields available for validation |
| SEG-02 | Application Processing | Validate identity/business data completeness | Intake package completed | Validated customer profile generated |
| SEG-03 | Eligibility Review | Assess risk and compliance acceptability | Validated profile available | Decision recommendation and reviewer action |
| SEG-04 | Final Decision | Complete onboarding decision and provisioning actions | Reviewer decision submitted | Case approved/rejected/returned with audit trail |

## 2) Segment Handoffs

| From Segment | To Segment | Handoff Artifact | Handoff Condition |
|---|---|---|---|
| SEG-01 | SEG-02 | Extracted data package + document metadata | Required files received and parsed |
| SEG-02 | SEG-03 | Verified customer profile + risk inputs | Identity and ownership checks completed |
| SEG-03 | SEG-04 | Reviewer decision payload + rationale | Decision action submitted |

## 3) Segment Risks

| Segment ID | Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|---|
| SEG-01 | OCR/extraction errors | Medium | Medium | Confidence thresholds + manual validation queue |
| SEG-02 | Conflicting ownership data | Medium | High | Agent-assisted anomaly checks + reviewer override |
| SEG-03 | False-positive screening hit | High | High | Dedicated adjudication task and evidence panel |
| SEG-04 | Downstream system update failure | Medium | Medium | API retry + RPA fallback for legacy core app |

## 4) Segment KPI Mapping

| Segment ID | KPI | Baseline | Target |
|---|---|---|---|
| SEG-01 | Intake completeness at first pass | 68% | 90% |
| SEG-02 | Validation cycle time | 4 hours | 45 minutes |
| SEG-03 | Manual review effort per case | 50 minutes | 20 minutes |
| SEG-04 | End-to-end onboarding cycle time | 2 business days | < 6 hours |

## 5) Segment Storyline For Demo

- Segment to highlight first: SEG-03 Eligibility Review
- Segment with strongest AI automation story: SEG-02 Application Processing
- Segment with strongest human-in-the-loop story: SEG-03 Eligibility Review
