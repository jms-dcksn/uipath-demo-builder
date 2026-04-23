# Agentic Business Logic Layer - KYC Onboarding

## 1) Logic Layer Purpose

- Business objective: Reduce onboarding lead time while preserving compliance rigor.
- Automation objective: Automate evidence extraction, validation, and pre-review triage.
- Human oversight objective: Keep accountable decisions and ambiguous risk resolution with designated reviewers.

## 2) Agent Responsibilities

| Agent/Service | Responsibility | Input Signals | Output Artifacts |
|---|---|---|---|
| Completeness Agent | Validate required data and quality | IDP fields, confidence, policy rules | Missing-field list and readiness score |
| Ownership Agent | Assess beneficial ownership structure | Owner list and percentages | Ownership pass/fail + rationale |
| Review Brief Agent | Summarize risk posture for reviewer | Screening outputs, notes, documents | Structured review brief |

## 3) Segment To Task Mapping

| Segment ID | Task ID | Task Name | Execution Type | Component Type | Owner |
|---|---|---|---|---|---|
| SEG-01 | T-001 | Capture application package | Human-in-loop | User Task + API | Operations |
| SEG-01 | T-002 | Extract KYC fields | Automated | IDP | Document AI Team |
| SEG-02 | T-003 | Validate completeness | Automated | AI Agent | AI Team |
| SEG-02 | T-004 | Verify registry data | Automated | API | Integration Team |
| SEG-02 | T-005 | Beneficial owner checks | Automated | AI Agent | AI Team |
| SEG-03 | T-006 | Sanctions screening | Automated | API | Integration Team |
| SEG-03 | T-007 | EDD summarization | Automated | AI Agent | AI Team |
| SEG-03 | T-008 | Reviewer adjudication | Human-in-loop | User Task | KYC/Compliance |
| SEG-04 | T-009 | Update core system | Automated | RPA | Automation Team |
| SEG-04 | T-010 | Notify stakeholders | Automated | API | Integration Team |
| SEG-04 | T-011 | Close case | Automated | API | Integration Team |

## 4) State Model

| State | Entry Criteria | Exit Criteria | Owner |
|---|---|---|---|
| New | Case created | Intake fields extracted | System |
| InReview | Validation and screening started | Final decision submitted | KYC Analyst |
| PendingInfo | Required info missing | Customer/ops provides missing data | Operations |
| Approved | Decision approved | Core updates and notifications completed | System |
| Rejected | Decision rejected | Notifications completed | System |
| Closed | Terminal tasks complete | N/A | System |

## 5) Decision Rules

| Rule ID | Description | Inputs | Logic | Fallback |
|---|---|---|---|---|
| R-001 | Completeness gate | Required fields + confidence | If any required field missing/low confidence -> fail | Create PendingInfo task |
| R-002 | Ownership structure check | Owners + percentages | If ownership criteria unmet -> escalate | Route to compliance review |
| R-003 | Screening escalation | Screening result severity | If strong/ambiguous hit -> manual adjudication | Senior reviewer escalation |
| R-004 | Fast-track eligibility | Risk score, hit status, completeness | If low risk and no hits -> fast-track | Standard review path |

## 6) Human In The Loop Design

- Task types requiring human review: T-008 adjudication, escalated ownership conflicts.
- Evidence shown to reviewer: document excerpts, screening details, agent rationale, full audit timeline.
- Approval/rejection semantics: Must include reason code and reviewer comment.
- Escalation trigger: High-risk score, unresolved screening ambiguity, or policy conflict.

## 7) Exception Handling

- Retry policy: 3 retries with exponential backoff for external APIs.
- Dead-letter handling: Failed integration events routed to ops queue.
- Manual intervention flow: Create exception task with suggested resolution steps.
- Data correction flow: Transition case to `PendingInfo` and reopen validation tasks.

## 8) Component Buildability Notes

- Components buildable directly here: Python AI agents, case entity JSON, frontend application.
- Components that require proprietary platform implementation: RPA workflows, IDP models, native case model configuration.
- Handoff artifacts required for proprietary components: component specs, task contracts, acceptance scenarios.

## 9) Observability Signals

| Signal | Event Name | Why It Matters | Dashboard Use |
|---|---|---|---|
| SLA breach warning | CaseSlaWarning | Proactive queue intervention | Highlight aging cases |
| Screening escalation rate | ScreeningEscalationRate | Detect risk pressure and false positives | Segment-level KPI card |
| API failure rate | IntegrationFailureRate | Monitor external dependency health | Ops alerting panel |

## 10) Non-Functional Requirements

- Throughput target: 500 new onboarding cases/day.
- Latency target: < 60 seconds for automated checks after document extraction.
- Availability target: 99.5% for orchestration and task queue.
- Auditability requirements: Immutable event timeline for all state transitions and decision actions.
