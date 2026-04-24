# Demo Script - KYC Onboarding

## 1) Demo Metadata

- Use case: US Commercial Banking KYC Onboarding
- Version: 1.0
- Author: Demo Architect
- Date: 2026-03-10
- Target audience: Banking operations leaders, compliance stakeholders
- Duration target (minutes): 8

## 2) Key Messages

| Message ID | Key Message | Business Proof Point | Related Requirement IDs |
|---|---|---|---|
| MSG-01 | Maestro gives operations teams immediate visibility into KYC workload and risk. | Dashboard highlights queue priority, stage, and blockers in one view. | BR-001, BR-003 |
| MSG-02 | Case Management adapts to exception-heavy KYC processes without forcing a rigid sequence. | Case timeline shows rework loops and adaptive task routing. | BR-004, BR-006 |
| MSG-03 | Agents + IDP + APIs produce decision-ready insights faster and with better traceability. | Detail page shows extracted fields, screening outcomes, and recommendation summary. | BR-005, BR-007 |
| MSG-04 | Human reviewers make final decisions with confidence using complete context. | Reviewer action updates case status and closes with full audit trail. | BR-008 |

## 3) Visual Inventory

| Visual ID | Visual Name | Location (UI/System) | What It Proves |
|---|---|---|---|
| VIS-01 | Dashboard worklist + KPIs | Frontend `/dashboard` | Unified workload and risk posture |
| VIS-02 | Filtered queue (`PendingInfo`) | Frontend `/dashboard` | Fast triage and prioritization |
| VIS-03 | Case detail header + status panels | Frontend `/instances/:instanceId` | Current stage and decision context |
| VIS-04 | Maestro case instance | UiPath Maestro | Adaptive lifecycle coordination |
| VIS-05 | Task execution summary | Frontend `/instances/:instanceId` | Agent/IDP/API collaboration outcomes |
| VIS-06 | Final review action panel | Frontend `/instances/:instanceId` | Human decision with auditability |

## 4) Message-To-Visual Alignment

| Message ID | Visual IDs (2-3) | Why This Pairing Works |
|---|---|---|
| MSG-01 | VIS-01, VIS-02 | Shows immediate visibility and prioritization behavior. |
| MSG-02 | VIS-03, VIS-04 | Connects user context to adaptive orchestration behavior. |
| MSG-03 | VIS-04, VIS-05 | Demonstrates cross-component intelligence and evidence synthesis. |
| MSG-04 | VIS-03, VIS-06 | Shows the final decision point and controlled closure path. |

## 5) Run Of Show

| Beat # | Message ID | Visual ID | Narration (What to Say) | Operator Action (What to Do) | Visible Outcome (What Audience Sees) |
|---|---|---|---|---|---|
| 1 | MSG-01 | VIS-01 | "This dashboard shows exactly what needs attention across KYC onboarding." | Open dashboard and pause on KPI strip and list. | Audience sees total volume, at-risk items, and current ownership. |
| 2 | MSG-01 | VIS-02 | "I can isolate pending information cases instantly and focus reviewer time where it matters." | Apply `PendingInfo` filter. | List narrows to cases blocked on missing documents. |
| 3 | MSG-02 | VIS-03 | "Let’s open one case to see what the reviewer needs to decide next." | Open selected instance. | Detail page shows stage, owner, task context, and evidence status. |
| 4 | MSG-02 | VIS-04 | "Behind this page, Case Management is coordinating non-linear rework and dependencies." | Switch to Maestro case instance view. | Audience sees adaptive progression and task history. |
| 5 | MSG-03 | VIS-05 | "Agents, IDP, and APIs have already prepared a clear recommendation package." | Return to detail and show execution summary/timeline. | Extracted data, screening status, and recommendation are visible together. |
| 6 | MSG-04 | VIS-06 | "With full context and traceability, the reviewer can decide confidently." | Submit approve action with reason code. | Status updates and closure path starts with audit events recorded. |

## 6) Opening And Close

- Opening value statement: "KYC is high-volume and exception-heavy; this demo shows how UiPath shortens cycle time while improving decision quality."
- Closing business impact statement: "By orchestrating people, systems, and AI in one adaptive flow, we reduce onboarding delays and make every decision audit-ready."

## 7) Contingency Lines

| Risk During Live Demo | Fallback Line | Fallback Action |
|---|---|---|
| Screening API delay | "We can continue with the pre-completed screening result to show the same decision path." | Open backup case pre-seeded with screening outputs. |
| Task list timeout | "The same state is visible in this saved case snapshot." | Refresh once, then switch to backup case instance. |
