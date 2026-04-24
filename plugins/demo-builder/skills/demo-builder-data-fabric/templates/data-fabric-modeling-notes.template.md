# Data Fabric Modeling Notes Template

## 1) Entity Boundary And Foldering

- Why this entity boundary was chosen:
- Why these fields are in the primary entity:
- What was intentionally excluded:
- Folder strategy (`folderId`) and environment isolation notes:

## 2) Field Shape Decisions

- Required fields and rationale (`isRequired`):
- Business uniqueness fields (`isUnique`) and collision strategy:
- SQL type selections (`sqlType.name`, `lengthLimit`, precision/range):
- JSON-in-`MULTILINE` contracts (list field names and expected JSON shape):
- Date/time strategy (`DATE` vs `DATETIMEOFFSET`, timezone assumptions):

## 3) Relationship And Referential Integrity

- Foreign keys (`isForeignKey`) and cardinality:
- Referenced entity and field contracts:
- Relationship field display behavior (`fieldDisplayType`) notes:
- Orphan-handling strategy when related records are missing:

## 4) Security And Access

- RBAC decision (`isRbacEnabled`) and ownership model:
- Sensitive/regulated fields:
- Encryption requirements (`isEncrypted`) and masking/tokenization needs:
- Audit requirements for who changed what and when:

## 5) Performance And Operations

- Query patterns expected:
- High-frequency filter/sort fields:
- Expected record volume and growth assumptions:
- Retention policy:
- Archival policy:
- Purge policy:

## 6) Integration Notes

- Fields consumed by Maestro:
- Fields consumed by frontend:
- Fields written by external integrations:
- Fields used for segment/task traceability:
- JSON serialization/deserialization responsibilities by component:

## 7) Import And Versioning Notes

- Schema artifact location:
- Import steps in target tenant:
- Post-import metadata expectations (`id`, `recordCount`, `createTime`, `updateTime`):
- Backward compatibility strategy:
- Migration notes for schema changes:

## 8) Reconciliation Notes

- Modeling pass: `initial` or `reconciled`
- Case Management artifacts reviewed:
- Stub contracts reviewed:
- Agent output contracts reviewed:
- Fields added during reconciliation:
- Outputs marked non-persistent:

## 9) Known Gaps

- Gap:
- Owner:
- Planned resolution:
