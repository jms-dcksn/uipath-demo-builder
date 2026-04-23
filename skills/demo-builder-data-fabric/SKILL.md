---
name: demo-builder-data-fabric
description: "Define the case entity model for a UiPath demo in Data Fabric. Maps business requirements to entity fields, produces a UiPath Data Fabric entity-definition JSON (name, displayName, fields, sqlType, FK references, RBAC flags), and generates a realistic example record aligned to the schema. Use when the demo needs a case data model for persistence. Typically invoked by demo-builder-planner after orchestration is chosen."
allowed-tools: Bash, Read, Write, Edit, Glob, Grep, AskUserQuestion
---

# Demo Builder — Data Fabric

Produce the case entity schema and example records for demo-level Data Fabric import. **Demo-grade, not production** — keep fields minimal and coherent with the orchestration + UI story.

## When to use

- After orchestration choice, when the demo needs a persistent case record.
- When the UI's `ui-data-contract` needs a backing schema.

## Inputs

- Task automation matrix (from `demo-builder-discovery` or planner)
- Orchestration design (BPMN or Case Management)

## Workflow

1. Fill `templates/requirements-to-case-entity.template.md` — each `BR-*` maps to one or more entity fields (traceability).
2. Produce `templates/case-entity.schema.template.json` — Data Fabric entity definition. Define for every field: `name` (camelCase), `displayName`, `sqlType`, nullability, uniqueness, FK refs, RBAC flags.
3. Produce `templates/case-entity.example.json` — a realistic example record. Field relationships must be coherent with the list + detail UI pages.
4. Fill `templates/data-fabric-modeling-notes.template.md` — modeling decisions, lifecycle/state field alignment, security/retention/integration assumptions.

## Demo rules

- Keep field count small. Only model what the story and UI need.
- Use realistic but anonymized example data.
- Pre-stage a small varied set (normal / urgent / exception) — see `demo-design-principles.md` from the planner skill.
- If the tenant-side import isn't available, mark the artifacts as "ready for import" and hand off.

## Completion criteria

- Every key `BR-*` maps to one or more fields.
- Field SQL types, nullability, uniqueness, FK behavior defined.
- Lifecycle/state fields align with the orchestration design.
- JSON artifacts are valid and ready for platform import.

## Templates

- `templates/requirements-to-case-entity.template.md`
- `templates/case-entity.schema.template.json`
- `templates/case-entity.example.json`
- `templates/data-fabric-modeling-notes.template.md`

## See also

- Production Data Fabric operations (entity CRUD, record ops, CSV import, attachments) → use the `uipath-data-fabric` skill directly.
