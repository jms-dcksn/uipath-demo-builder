# Task Automation Matrix - KYC Onboarding

## 1) Task Matrix


| Segment ID | Task ID | Task Name                            | Purpose                                               | Execution Type | Component      | Input                               | Output                               | Exception Path                        |
| ---------- | ------- | ------------------------------------ | ----------------------------------------------------- | -------------- | -------------- | ----------------------------------- | ------------------------------------ | ------------------------------------- |
| SEG-01     | T-001   | Capture application package          | Register application and files                        | Human Task     | Frontend + API | Submitted form and docs             | Case created with document pointers  | Prompt user to upload missing docs    |
| SEG-01     | T-002   | Extract KYC fields                   | Parse docs into structured fields                     | Automated      | IDP            | Formation docs, IDs                 | Extracted fields with confidence     | Route low-confidence fields to review |
| SEG-02     | T-003   | Validate completeness                | Check required fields and formats                     | Automated      | AI Agent       | Extracted fields                    | Validation result + missing list     | Create PendingInfo task               |
| SEG-02     | T-004   | Verify registry data                 | Compare business details to registry records          | Automated      | API            | Entity name, registration number    | Match/no-match flags                 | Retry then escalate if unavailable    |
| SEG-02     | T-005   | Beneficial owner checks              | Validate ownership thresholds and required attributes | Automated      | AI Agent       | Beneficial owner list               | Ownership compliance assessment      | Escalate to compliance review         |
| SEG-03     | T-006   | Sanctions screening                  | Screen parties against sanctions lists                | Automated      | API            | Customer + owners + signatories     | Screening results and potential hits | Manual adjudication task              |
| SEG-03     | T-007   | Enhanced due diligence summarization | Summarize risk signals for reviewer                   | Automated      | AI Agent       | Screening outputs, documents, notes | Reviewer briefing                    | Route to senior reviewer if high risk |
| SEG-03     | T-008   | Reviewer adjudication                | Accept/reject/request info decision                   | Human Task     | Case task      | Full case context                   | Decision + reason code               | Escalation if ambiguous hit remains   |
| SEG-04     | T-009   | Update core system                   | Create/update customer profile in legacy core         | Automated      | RPA            | Approved case payload               | Core update confirmation             | Retry and create ops task             |
| SEG-04     | T-010   | Notify stakeholders                  | Send onboarding result and next steps                 | Automated      | API            | Final decision payload              | Notifications dispatched             | Queue retry if email/API failure      |
| SEG-04     | T-011   | Close case                           | Mark case terminal status and timestamps              | Automated      | API            | Decision metadata                   | Case status Closed                   | Raise support task on failure         |


## 2) Task Dependencies


| Task ID | Depends On   | Dependency Type             |
| ------- | ------------ | --------------------------- |
| T-002   | T-001        | Document availability       |
| T-003   | T-002        | Data prerequisite           |
| T-004   | T-003        | Validation prerequisite     |
| T-005   | T-003        | Data prerequisite           |
| T-006   | T-004, T-005 | Composite prerequisite      |
| T-007   | T-006        | Analysis prerequisite       |
| T-008   | T-007        | Human decision prerequisite |
| T-009   | T-008        | Approval prerequisite       |
| T-010   | T-008        | Decision prerequisite       |
| T-011   | T-009, T-010 | Completion prerequisite     |


## 3) Automation Fit Notes

- `AI Agent`: validation logic, anomaly detection, and reviewer briefing generation.
- `IDP`: extraction from submitted legal and identity documents.
- `RPA`: core banking system update where modern API is unavailable.
- `API`: sanctions screening, notifications, and case status transitions.
- `Human Task`: policy-accountable final decision and ambiguous screening resolution.

## 4) Task-Level Non-Functional Targets


| Task ID | SLA                       | Accuracy Target                  | Audit Requirement                                |
| ------- | ------------------------- | -------------------------------- | ------------------------------------------------ |
| T-002   | 3 min per document bundle | >= 95% critical field extraction | Persist confidence per field                     |
| T-003   | < 30 sec                  | >= 98% required-field detection  | Store rule outcomes and reasons                  |
| T-006   | < 60 sec per party        | 100% list check completion       | Store list version + timestamp                   |
| T-008   | 8 business hours          | N/A                              | Store reviewer ID, rationale, and evidence links |


