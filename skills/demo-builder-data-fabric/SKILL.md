---
name: demo-builder-data-fabric
description: "Define and reconcile the case entity model for a UiPath Case Management demo in Data Fabric. Maps business requirements, case stages, stub outputs, agent outputs, and UI fields to entity fields; produces a UiPath Data Fabric entity-definition JSON; and generates realistic normal/urgent/exception records aligned to the schema. Typically invoked by demo-builder-planner before and after Case Management design."
allowed-tools: Bash, Read, Write, Edit, Glob, Grep, AskUserQuestion
---

# Demo Builder — Data Fabric

Produce the case entity schema and example records for demo-level Data Fabric import. **Demo-grade, not production** — keep fields minimal and coherent with the orchestration + UI story.

## When to use

- Before Case Management design, when the demo needs an initial persistent case record.
- After Case Management design, when stub/agent/human-task outputs must be reconciled to entity fields.
- When the UI's `ui-data-contract` needs a backing schema.

## Inputs

- Task automation matrix (from `demo-builder-discovery` or planner)
- Case Management design and stub contracts, when running in reconciliation mode
- Agent output contracts, when available

## Workflow

1. Copy `templates/requirements-to-case-entity.template.md` to `builds/<demo-slug>/case-entity/requirements-to-case-entity.md` and fill it. Each `BR-*` maps to one or more entity fields.
2. Copy `templates/case-entity.schema.template.json` to `builds/<demo-slug>/case-entity/case-entity.schema.json` and produce the Data Fabric entity definition. Define for every field: `name` (camelCase), `displayName`, `sqlType`, nullability, uniqueness, FK refs, RBAC flags.
3. Copy `templates/case-entity.example.json` to `builds/<demo-slug>/case-entity/case-entity.example.json` and produce at least three coherent example records: normal, urgent, and exception.
4. Copy `templates/data-fabric-modeling-notes.template.md` to `builds/<demo-slug>/case-entity/data-fabric-modeling-notes.md` and fill it with modeling decisions, lifecycle/state field alignment, security/retention/integration assumptions, and reconciliation notes.
5. In reconciliation mode, compare Case Management stub outputs, human-task outputs, and agent output contracts against the schema:
   - Add missing persisted fields.
   - Mark non-persistent outputs explicitly.
   - Update all example records.
   - Record the reconciliation result in `data-fabric-modeling-notes.md`.

## Demo rules

- Keep field count small. Only model what the story and UI need.
- Use realistic but anonymized example data.
- Pre-stage a small varied set (normal / urgent / exception) — see `demo-design-principles.md` from the planner skill.
- If the tenant-side import isn't available, mark the artifacts as "ready for import" and hand off.

## Completion criteria

- Every key `BR-*` maps to one or more fields.
- Every persisted output from a trigger, RPA/API/IDP stub, human task, or agent maps to a field.
- Any non-persistent output is explicitly marked as non-persistent.
- Field SQL types, nullability, uniqueness, FK behavior defined.
- Lifecycle/state fields align with the orchestration design.
- JSON artifacts are valid and ready for platform import.
- Normal, urgent, and exception example records are coherent with the schema.

## Templates

- `templates/requirements-to-case-entity.template.md`
- `templates/case-entity.schema.template.json`
- `templates/case-entity.example.json`
- `templates/data-fabric-modeling-notes.template.md`

## See also

- Production Data Fabric operations (entity CRUD, record ops, CSV import, attachments) → use the `uipath-data-fabric` skill directly.
