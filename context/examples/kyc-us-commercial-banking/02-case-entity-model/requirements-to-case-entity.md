# Requirements To Case Entity Mapping - KYC Onboarding

## 1) Entity Overview

- Entity logical name (`name`): `KycOnboardingCase`
- Entity display name (`displayName`): `KYC_Onboarding_Case`
- Entity purpose/description: Persist adaptive case-management state and procedural BPMN handoff context for KYC onboarding.
- Entity type (`entityType`): `Entity`
- Folder ID (`folderId`): `00000000-0000-0000-0000-000000000000`
- Primary business key field: `caseid`
- Natural key (if any): `applicationreference`
- Orchestration model (`BPMN` or `Case Management`): `Case Management` (primary), `BPMN` (procedural variant)

## 2) Requirement Traceability

| Requirement ID | Requirement Summary | Segment/Task IDs | Schema Field Name | Display Name | SQL Type | UI Consumer(s) | Notes |
|---|---|---|---|---|---|---|---|
| BR-001 | Track lifecycle status and queue state | SEG-01..SEG-04 | status, currentsegmentid, currenttaskid | status, current_segment_id, current_task_id | NVARCHAR | Dashboard + instance detail | Canonical status and work-position fields |
| BR-002 | Store legal and registration identity context | SEG-01/T-001, SEG-02/T-004 | legalentityname, registrationnumber, incorporationcountry | legal_entity_name, registration_number, incorporation_country | NVARCHAR | Dashboard + instance detail | Core reviewer context |
| BR-003 | Capture beneficial ownership structure | SEG-02/T-005 | beneficialownercount, beneficialowners | beneficial_owner_count, beneficial_owners | DECIMAL + MULTILINE | Instance detail | JSON payload in multiline text |
| BR-004 | Persist screening and risk outcomes | SEG-03/T-006, SEG-03/T-007 | screeningstatus, screeningsummary, riskscore, risktier | screening_status, screening_summary, risk_score, risk_tier | NVARCHAR + MULTILINE + DECIMAL | Dashboard + instance detail | Drives triage and adjudication |
| BR-005 | Record accountable human decision | SEG-03/T-008 | decision, decisionreasoncode, reviewedby, reviewercomment, decisionat | decision, decision_reason_code, reviewed_by, reviewer_comment, decision_at | NVARCHAR + MULTILINE + DATETIMEOFFSET | Instance detail | Human accountability for audit |
| BR-006 | Support operational ownership and SLA tracking | SEG-03/T-008, SEG-04/T-011 | assignedto, assignedteam, createdat, updatedat, closedat | assigned_to, assigned_team, created_at, updated_at, closed_at | NVARCHAR + DATETIMEOFFSET | Dashboard + instance detail | Queue ownership + latency metrics |
| BR-007 | Preserve timeline and task snapshots for demo storytelling | All | audittrail, tasksnapshots | audit_trail, task_snapshots | MULTILINE | Instance detail | JSON arrays rendered as timeline/task panels |

## 3) Field Definitions (UiPath Data Fabric)

| Field Name | Display Name | SQL Type | Length / Precision | Required | Unique | Foreign Key | Encrypted | JSON/Text Contract | Description |
|---|---|---|---|---|---|---|---|---|---|
| caseid | case_id | NVARCHAR | 40 | Yes | Yes | No | No | n/a | Unique case identifier |
| applicationreference | application_reference | NVARCHAR | 64 | Yes | Yes | No | No | n/a | Intake channel reference |
| status | status | NVARCHAR | 32 | Yes | No | No | No | n/a | Canonical lifecycle state |
| beneficialowners | beneficial_owners | MULTILINE | 10000 | Yes | No | No | No | JSON array string | Beneficial owner records |
| riskscore | risk_score | DECIMAL | precision 2, range 0..100 | No | No | No | No | n/a | Computed risk score |
| reviewercomment | reviewer_comment | MULTILINE | 4000 | No | No | No | No | Free-text adjudication rationale | Reviewer explanation |
| onboardingbatch | onboarding_batch | UNIQUEIDENTIFIER | 300 | No | No | Yes | No | FK to batch entity | Optional batch grouping |

## 4) Relationship Design

| Relationship Field | Cardinality | Reference Entity | Reference Field | SQL Type | Purpose |
|---|---|---|---|---|---|
| onboardingbatch | N:1 | KycOnboardingBatch | batchid | UNIQUEIDENTIFIER | Group related onboarding cases for bulk operations/demo segmentation |

## 5) Orchestration Stage Alignment

| Orchestration State/Stage | Required Field Updates | Updated By | Validation Rule |
|---|---|---|---|
| Intake | caseid, applicationreference, status, createdat | API + IDP | `caseid` unique and required fields populated |
| Processing | screeningstatus, screeningsummary, riskscore, risktier, updatedat | AI Agent + API | risk score range and screening status enum checks |
| Review | decision, decisionreasoncode, reviewedby, reviewercomment, decisionat, updatedat | Human Task | reason code required for non-auto decisions |
| Closure | status, closedat, updatedat | API | closure requires terminal status |

## 6) Data Quality Rules

- Rule ID: DQ-001
- Fields: `registrationnumber`
- Validation strategy: Regex + registry API parity check.
- Error handling strategy: Route to review exception task with correction guidance.

- Rule ID: DQ-002
- Fields: `beneficialownercount`, `beneficialowners`
- Validation strategy: Parse JSON and validate row count parity.
- Error handling strategy: Mark case `PendingInfo` and generate human follow-up task.

## 7) Lifecycle Write Matrix

| Lifecycle Step | Fields Created | Fields Updated | Writer Type (`AI Agent`, `RPA`, `IDP`, `API`, `Human Task`) |
|---|---|---|---|
| Case creation | caseid, applicationreference, createdat | status, updatedat | API + IDP |
| Automated enrichment | beneficialowners, screeningstatus, screeningsummary, riskscore | risktier, updatedat, tasksnapshots | AI Agent + API |
| Human adjudication | decision, decisionreasoncode, reviewedby, reviewercomment, decisionat | status, assignedto, assignedteam, updatedat, audittrail | Human Task |
| Closure/finalization | closedat | status, updatedat, audittrail | API |

## 8) Import/Export Readiness

- `invalidIdentifiers` reviewed and resolved: Yes
- `isRbacEnabled` decision documented: Yes (`false` for demo portability)
- System-managed metadata excluded from manual edits (`id`, `createTime`, `updateTime`): Yes
- Post-import validation complete (field names, sql types, required flags, FK references): Yes (schema-level checklist complete; tenant import verification pending)
