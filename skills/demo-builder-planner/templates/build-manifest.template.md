# Build Manifest

## 1) Build Metadata

- Demo slug:
- Customer/account:
- Use case:
- Case type:
- Build owner:
- Last updated:

## 2) Phase Status

| Phase | Owner Skill/Agent | Status | Primary Artifact(s) | Validation |
|---|---|---|---|---|
| Preflight | demo-builder-planner | PENDING | CLI version/login status | Pending |
| Discovery | demo-builder-discovery | PENDING | `discovery/use-case-research.md`, `discovery/source-register.md`, `discovery/segment-map.md`, `discovery/task-automation-matrix.md` | Pending |
| Initial data model | data-modeler | PENDING | `case-entity/case-entity.schema.json`, `case-entity/case-entity.example.json` | Pending |
| Case Management design | case-designer | PENDING | `flow-model/case-management-design.md`, `flow-model/sdd.md` | Pending |
| Caseplan generation | uipath-case-management | PENDING | `flow-model/tasks.md`, `flow-model/caseplan.json` | Pending |
| Data model reconciliation | data-modeler | PENDING | `case-entity/data-fabric-modeling-notes.md` | Pending |
| Agents | agent-builder | PENDING | `agents/<AG-id>/` | Pending |
| Fixture consistency | demo-builder-planner | PENDING | `case-entity/fixture-consistency.md` | Pending |
| Frontend | frontend-builder | PENDING | `frontend/` | Pending |
| Frontend-schema reconcile | demo-builder-planner | PENDING | `case-entity/data-fabric-modeling-notes.md`, `frontend/src/types/` | Pending |
| Manual checklist | demo-builder-planner | PENDING | `manual-completion-checklist.md` | Pending |
| Demo script | demo-builder-script | PENDING | `script/demo-script.md` | Pending |

## 3) Artifact Register

| Artifact | Path | Produced By | Depends On | Status |
|---|---|---|---|---|
| Source register | `discovery/source-register.md` | demo-builder-discovery | User docs + web sources | PENDING |
| Case entity schema | `case-entity/case-entity.schema.json` | data-modeler | Task matrix, case design | PENDING |
| Case plan | `flow-model/caseplan.json` | uipath-case-management | `sdd.md`, user approval | PENDING |
| Agent project | `agents/<AG-id>/` | agent-builder | Agent build spec | PENDING |
| Coded Web App | `frontend/` | frontend-builder | UI data contract | PENDING |

## 4) Validation Register

| Check | Command or Review | Result | Notes |
|---|---|---|---|
| JSON schema valid | `json` parser or platform import check | Pending |  |
| Case Management generated | `uip maestro case validate` via uipath-case-management | Pending |  |
| Agent local run | `uip codedagent run <entrypoint> '<input-json>'` | Pending | One row per agent in notes |
| Agent smoke eval | `uip codedagent eval <entrypoint> evaluations/eval-sets/smoke-test.json --no-report` | Pending | One row per agent in notes |
| Frontend build | `npm run build` | Pending |  |
| Demo script dry run | Manual rehearsal | Pending |  |

## 5) Open Issues

| ID | Issue | Owner | Target Resolution | Status |
|---|---|---|---|---|
| OI-001 |  |  |  | Open |
