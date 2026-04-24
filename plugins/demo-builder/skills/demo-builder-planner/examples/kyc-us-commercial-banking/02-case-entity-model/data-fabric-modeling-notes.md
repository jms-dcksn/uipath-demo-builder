# Data Fabric Modeling Notes - KYC Onboarding

## 1) Entity Boundary And Foldering

- Why this entity boundary was chosen: KYC onboarding is case-centric and requires one durable record for iterative review, exception handling, and final decision traceability.
- Why these fields are in the primary entity: They provide the minimum viable operational, compliance, and storytelling context for dashboard plus instance detail pages.
- What was intentionally excluded: Raw document binaries, full third-party API payloads, and verbose system logs (retained in source systems).
- Folder strategy (`folderId`) and environment isolation notes: Use default folder in example; target tenants should separate dev/test/prod folders.

## 2) Field Shape Decisions

- Required fields and rationale (`isRequired`): `caseid`, `applicationreference`, `status`, `industry`, `domain`, `legalentityname`, `registrationnumber`, `incorporationcountry`, `beneficialownercount`, `beneficialowners`, `createdat`, `updatedat` to ensure deterministic minimum context.
- Business uniqueness fields (`isUnique`) and collision strategy: `caseid` and `applicationreference` are unique; duplicates route to intake exception handling.
- SQL type selections (`sqlType.name`, `lengthLimit`, precision/range): IDs and categorical values use `NVARCHAR`; score/count values use `DECIMAL`; timestamps use `DATETIMEOFFSET`; nested structures use `MULTILINE` JSON strings.
- JSON-in-`MULTILINE` contracts (list field names and expected JSON shape): `beneficialowners`, `audittrail`, `tasksnapshots` are JSON arrays serialized as text.
- Date/time strategy (`DATE` vs `DATETIMEOFFSET`, timezone assumptions): `DATETIMEOFFSET` used for all lifecycle timestamps in UTC (`Z`) format.

## 3) Relationship And Referential Integrity

- Foreign keys (`isForeignKey`) and cardinality: `onboardingbatch` is optional `N:1` to `KycOnboardingBatch`.
- Referenced entity and field contracts: `onboardingbatch` references `KycOnboardingBatch.batchid`.
- Relationship field display behavior (`fieldDisplayType`) notes: Relationship rendered as selector in platform UI and as metadata value in demo detail page.
- Orphan-handling strategy when related records are missing: Allow null FK; surface informational warning only.

## 4) Security And Access

- RBAC decision (`isRbacEnabled`) and ownership model: Entity-level RBAC disabled in example for portability; production should enable role- and team-based restrictions.
- Sensitive/regulated fields: Beneficial owner payloads and reviewer comments.
- Encryption requirements (`isEncrypted`) and masking/tokenization needs: Keep unencrypted in demo tenant; mask owner details in dashboard and non-review roles.
- Audit requirements for who changed what and when: Persist user/task origin in `audittrail` and keep `updatedat` current.

## 5) Performance And Operations

- Query patterns expected: Dashboard filters on `status`, `risktier`, `assignedto`, `updatedat`; detail lookup by `caseid`.
- High-frequency filter/sort fields: `status`, `risktier`, `assignedto`, `updatedat`, `currentsegmentid`.
- Expected record volume and growth assumptions: 100k+ cases/year in medium volume scenario.
- Retention policy: Retain case metadata per compliance policy window.
- Archival policy: Move terminal records to archive store after operational period.
- Purge policy: Controlled purge via legal/compliance retention schedule.

## 6) Integration Notes

- Fields consumed by Maestro: `status`, `currentsegmentid`, `currenttaskid`, `decision`, `decisionreasoncode`.
- Fields consumed by frontend: Dashboard queue fields + full instance detail panels (metadata, variables, timeline).
- Fields written by external integrations: Screening outputs, registry validation indicators, notification outcomes.
- Fields used for segment/task traceability: `currentsegmentid`, `currenttaskid`, `tasksnapshots`.
- JSON serialization/deserialization responsibilities by component: AI/API writers serialize multiline JSON; frontend detail view parses with fallback to raw text.

## 7) Import And Versioning Notes

- Schema artifact location: `skills/demo-builder-planner/examples/kyc-us-commercial-banking/02-case-entity-model/case-entity.schema.json`
- Import steps in target tenant: Validate field names/types, import entity, seed `case-entity.example.json`, verify read/write from SDK.
- Post-import metadata expectations (`id`, `recordCount`, `createTime`, `updateTime`): Platform-managed and excluded from manual authoring.
- Backward compatibility strategy: Additive changes only; do not rename existing field identifiers.
- Migration notes for schema changes: Introduce new fields, backfill with scripts, and version UI/API contracts in lockstep.

## 8) Known Gaps

- Gap: Final adverse media provider contract is not finalized for production schema enrichment.
- Owner: Integration lead.
- Planned resolution: Confirm provider payload and add additional multiline fields in next schema revision.
