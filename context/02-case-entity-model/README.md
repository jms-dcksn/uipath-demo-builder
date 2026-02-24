# Case Entity Model

This partition maps business requirements to a structured case entity suitable for UiPath Data Fabric.

## Inputs

- `context/00-discovery-and-segmentation/task-automation-matrix.template.md`
- `context/01-use-case-agentic-logic/maestro-design.template.md` or `context/01-use-case-agentic-logic/case-management-design.template.md`

## Files

- `requirements-to-case-entity.template.md`: traceability from requirement to entity field.
- `case-entity.schema.template.json`: UiPath Data Fabric entity-definition template (`name`, `displayName`, `fields`, `sqlType`, FK references, RBAC flags).
- `case-entity.example.json`: realistic example record payload aligned to the schema field set.
- `data-fabric-modeling-notes.template.md`: modeling decisions, constraints, and operations notes.

## Completion Criteria

- Every key business requirement maps to one or more entity attributes.
- Field SQL types, nullability, uniqueness, and FK behavior are defined.
- Lifecycle/state fields align with orchestration design.
- Security, retention, and integration assumptions are documented.
- JSON artifacts are ready for platform import/adaptation.
