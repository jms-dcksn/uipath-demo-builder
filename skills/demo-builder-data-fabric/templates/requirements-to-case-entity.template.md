# Requirements To Case Entity Mapping Template

## 1) Entity Overview

- Entity logical name (`name`):
- Entity display name (`displayName`):
- Entity purpose/description:
- Entity type (`entityType`):
- Folder ID (`folderId`):
- Primary business key field:
- Natural key (if any):
- Orchestration model (`BPMN` or `Case Management`):

## 2) Requirement Traceability

| Requirement ID | Requirement Summary | Segment/Task IDs | Schema Field Name | Display Name | SQL Type | UI Consumer(s) | Notes |
|---|---|---|---|---|---|---|---|
| BR-001 | Track workflow stage | SEG-02/T-004 | stage | stage | NVARCHAR(32) | Dashboard + detail | Must align with orchestration stages |

## 3) Field Definitions (UiPath Data Fabric)

| Field Name | Display Name | SQL Type | Length / Precision | Required | Unique | Foreign Key | Encrypted | JSON/Text Contract | Description |
|---|---|---|---|---|---|---|---|---|---|
| reqid | req_id | NVARCHAR | 32 | Yes | Yes | No | No | n/a | Unique business identifier |
| stage | stage | NVARCHAR | 32 | Yes | No | No | No | n/a | Current workflow stage |
| examdetails | exam_details | MULTILINE | 10000 | No | No | No | No | JSON object/array string | Exam-level structured details |
| requisitiongroup | requisition_group | UNIQUEIDENTIFIER | 300 | No | No | Yes | No | FK to group entity | Parent/group relationship |

## 4) Relationship Design

| Relationship Field | Cardinality | Reference Entity | Reference Field | SQL Type | Purpose |
|---|---|---|---|---|---|
| requisitiongroup | N:1 | MIRRequisitionGroup | requisitiongroupid | UNIQUEIDENTIFIER | Group co-requested requisitions |

## 5) Orchestration Stage Alignment

| Orchestration State/Stage | Required Field Updates | Updated By | Validation Rule |
|---|---|---|---|
| Intake | reqid, stage, datecreated | API / IDP | `reqid` unique and `stage` not null |
| Review | stage, airecommendations, comments | AI Agent + Human Task | JSON payload valid for multiline fields |
| Booking | stage, bookinginfo, facilityoptions | API / Human Task | Booking/facility payload populated before scheduling |

## 6) Data Quality Rules

- Rule ID:
- Fields:
- Validation strategy:
- Error handling strategy:

## 7) Lifecycle Write Matrix

| Lifecycle Step | Fields Created | Fields Updated | Writer Type (`AI Agent`, `RPA`, `IDP`, `API`, `Human Task`) |
|---|---|---|---|
| Intake | reqid, stage, datecreated | datereceived | API |
| Clinical review | comments | stage, airecommendations, aisuggestions | AI Agent + Human Task |
| Scheduling | bookinginfo | stage, facilityoptions | API + Human Task |

## 8) Import/Export Readiness

- `invalidIdentifiers` reviewed and resolved:
- `isRbacEnabled` decision documented:
- System-managed metadata excluded from manual edits (`id`, `createTime`, `updateTime`):
- Post-import validation complete (field names, sql types, required flags, FK references):
