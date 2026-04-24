# Manual Completion Checklist

Use this as the final handoff for tenant-side items that cannot be completed from local demo artifacts.

## 1) Data Fabric

| Item | Required Value | Status | Owner | Notes |
|---|---|---|---|---|
| Target folder | folder name / folder ID | Pending |  |  |
| Entity imported | entity name | Pending |  | Import `case-entity/case-entity.schema.json` |
| Fixture records loaded | normal / urgent / exception | Pending |  | Import or manually create from examples |
| Field reconciliation reviewed | yes/no | Pending |  | Every persisted output maps to a field |

## 2) Case Management

| Item | Required Value | Status | Owner | Notes |
|---|---|---|---|---|
| `caseplan.json` validated | path / validation result | Pending |  |  |
| Solution/project created | Studio Web URL or project ID | Pending |  |  |
| Skeleton tasks resolved | `CMP-*` list | Pending |  | Attach real resources or keep demo stubs |
| Trigger configured | `CMP-TRG-01` | Pending |  | Email/form/webhook/queue/manual |
| Case published/deployed | environment/folder | Pending |  | Hand off to `uipath-platform` |

## 3) Agents

| Agent ID | Project Path | Studio Web Project ID | Local Run | Smoke Eval | Deploy Status | Manual Inputs Needed |
|---|---|---|---|---|---|---|
| AG-001 | `agents/AG-001/` | TBD | Pending | Pending | Pending | Context index, MCP URL, credentials |

## 4) Coded Web App

| Item | Required Value | Status | Owner | Notes |
|---|---|---|---|---|
| External Application created | client ID | Pending |  | Use required OAuth scopes |
| Redirect URI configured | local + deployed URL | Pending |  |  |
| `.env` populated | `VITE_UIPATH_*` values | Pending |  | Do not commit secrets |
| `npm run build` passes | build output | Pending |  |  |
| App packed | `.uipath/*.nupkg` | Pending |  | `uip codedapp pack` |
| App published | package/version | Pending |  | `uip codedapp publish` |
| App deployed | app URL | Pending |  | `uip codedapp deploy` |

## 5) Non-Agent Components

| Component ID | Type | Backing Task | Demo Stub Status | Real-Build Owner | Manual Completion Step |
|---|---|---|---|---|---|
| CMP-TRG-01 | Trigger | Case start | Pending |  | Configure trigger and payload |
| CMP-IDP-01 | IDP |  | Pending |  | Create/attach IDP project |
| CMP-API-01 | API |  | Pending |  | Configure connection or mock endpoint |
| CMP-RPA-01 | RPA |  | Pending |  | Build process and attach to skeleton task |

## 6) Demo Readiness

| Item | Status | Notes |
|---|---|---|
| Normal path fixture tested | Pending |  |
| Urgent path fixture tested | Pending |  |
| Exception path fixture tested | Pending |  |
| Demo script dry run completed | Pending |  |
| Fallback narration prepared | Pending |  |

