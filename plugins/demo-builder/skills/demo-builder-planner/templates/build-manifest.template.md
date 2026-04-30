# Build Manifest

## 1) Build Metadata

- Demo slug:
- Customer/account:
- Use case:
- Flow solution:
- Flow project:
- Build owner:
- Last updated:

## 2) Phase Status

| Phase | Owner Skill/Agent | Status | Primary Artifact(s) | Validation |
|---|---|---|---|---|
| Preflight | demo-builder-planner | PENDING | CLI version/login status | Pending |
| Discovery | demo-builder-discovery | PENDING | `discovery/use-case-research.md`, `discovery/source-register.md`, `discovery/process-map.md`, `discovery/task-automation-matrix.md` | Pending |
| Flow architecture | demo-builder-flow | PENDING | `flow/flow-architecture.md`, `flow/node-contracts.md`, `flow/registry-discovery.md`, `flow/connector-bindings.md` | Pending |
| Agents | agent-builder / demo-builder-agents | PENDING | `agents/<AG-id>/` | Pending |
| Flow build | demo-builder-flow | PENDING | `flow/<Solution>/<Project>/<Project>.flow` | Pending |
| Validate and tidy | demo-builder-flow | PENDING | validation/tidy output | Pending |
| Studio Web upload | demo-builder-planner | PENDING | Studio Web URL, SolutionId, upload response | Pending |
| Manual checklist | demo-builder-planner | PENDING | `handoff/manual-completion-checklist.md` | Pending |
| Demo script | demo-builder-script | PENDING | `script/demo-script.md` | Pending |

## 3) Artifact Register

| Artifact | Path | Produced By | Depends On | Status |
|---|---|---|---|---|
| Source register | `discovery/source-register.md` | demo-builder-discovery | User docs + web sources | PENDING |
| Flow architecture | `flow/flow-architecture.md` | demo-builder-flow | Task matrix | PENDING |
| Node contracts | `flow/node-contracts.md` | demo-builder-flow | Flow architecture | PENDING |
| Connector bindings | `flow/connector-bindings.md` | demo-builder-flow | Integration Service discovery | PENDING |
| Flow project | `flow/<Solution>/<Project>/<Project>.flow` | demo-builder-flow | Registry discovery | PENDING |
| Agent project | `agents/<AG-id>/` | agent-builder | Agent build spec | PENDING |
| Studio Web upload response | `manifest.md` | demo-builder-planner | Validate and tidy | PENDING |

## 4) Validation Register

| Check | Command or Review | Result | Notes |
|---|---|---|---|
| Flow validate before tidy | `uip maestro flow validate <file> --output json` | Pending |  |
| Flow tidy | `uip maestro flow tidy <file> --output json` | Pending |  |
| Flow validate after tidy | `uip maestro flow validate <file> --output json` | Pending |  |
| Agent validation/local run | agent-specific command | Pending | One row per agent in notes |
| Connector connection health | `uip is connections list/ping` | Pending | One row per connector |
| Tool/control node registry check | `uip maestro flow registry get <nodeType> --output json` | Pending | One row per local node family in notes |
| Studio Web upload | `uip solution upload <SolutionDir> --output json` | Pending | Record Studio Web URL, SolutionId, and uploaded projects |
| Demo script dry run | Manual rehearsal | Pending |  |

## 5) Studio Web Handoff

- Studio Web URL:
- SolutionId:
- Uploaded project names:
- Uploaded project IDs:
- Upload command:
- Upload status:
- Omitted expected projects:

## 6) Open Issues

| ID | Issue | Owner | Target Resolution | Status |
|---|---|---|---|---|
| OI-001 |  |  |  | Open |
