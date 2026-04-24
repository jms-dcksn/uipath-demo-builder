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
| Discovery | demo-builder-discovery | Planned | `discovery/use-case-research.md`, `discovery/source-register.md`, `discovery/segment-map.md`, `discovery/task-automation-matrix.md` | Pending |
| Initial data model | data-modeler | Planned | `case-entity/case-entity.schema.json`, `case-entity/case-entity.example.json` | Pending |
| Case Management | case-designer | Planned | `flow-model/case-management-design.md`, `flow-model/sdd.md`, `flow-model/tasks.md`, `flow-model/caseplan.json` | Pending |
| Data model reconciliation | data-modeler | Planned | `case-entity/data-fabric-modeling-notes.md` | Pending |
| Agents | agent-builder | Planned | `agents/<AG-id>/` | Pending |
| Frontend | frontend-builder | Planned | `frontend/` | Pending |
| Manual checklist | demo-builder-planner | Planned | `manual-completion-checklist.md` | Pending |
| Demo script | demo-builder-script | Planned | `script/demo-script.md` | Pending |

## 3) Artifact Register

| Artifact | Path | Produced By | Depends On | Status |
|---|---|---|---|---|
| Source register | `discovery/source-register.md` | demo-builder-discovery | User docs + web sources | Planned |
| Case entity schema | `case-entity/case-entity.schema.json` | data-modeler | Task matrix, case design | Planned |
| Case plan | `flow-model/caseplan.json` | uipath-case-management | `sdd.md`, user approval | Planned |
| Agent project | `agents/<AG-id>/` | agent-builder | Agent build spec | Planned |
| Coded Web App | `frontend/` | frontend-builder | UI data contract | Planned |

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

