# Delivery Backlog Template

## 1) Milestone Plan

| Milestone | Goal | Exit Criteria | Target Date |
|---|---|---|---|
| M1 | Discovery complete | Source register, process map, and task matrix approved |  |
| M2 | Mock API and Flow architecture complete | Payload field map, API contracts, node contracts, and resource discovery reviewed |  |
| M3 | Flow assets complete | Flow, agents, API workflows, manual-start input contract, connector prerequisites, and local tool/control nodes resolved or documented |  |
| M4 | Studio Web handoff complete | Validation passed, input surface verified or flagged, solution uploaded, URL captured, and script dry-run completed |  |

## 2) Work Items

| Item ID | Type | Description | Depends On | Owner | Status |
|---|---|---|---|---|---|
| WI-001 | Discovery | Draft Flow-oriented task matrix |  |  | Planned |
| WI-002 | Mock API Workflow | Create and validate API-* payloads and passthrough workflows | WI-001 |  | Planned |
| WI-003 | Flow | Build and validate Maestro Flow | WI-001, WI-002 |  | Planned |
| WI-004 | Studio Web Upload | Upload solution and verify manual-start input surface | WI-003 |  | Planned |
| WI-005 | Demo Script | Create run-of-show with Flow proof points | WI-004 |  | Planned |

## 3) Ownership Model

- Product/demo owner:
- Process SME:
- Flow owner:
- Agent owner:
- API workflow owner:
- Integration owner:
- Demo narrator/operator:

## 4) Risks And Mitigations

| Risk | Probability | Impact | Mitigation | Owner |
|---|---|---|---|---|
| Connector connection unavailable | Medium | High | Use fixture or alternate connector; document prerequisite |  |
| API workflow cannot be registered before demo | Medium | Medium | Use documented fixture-backed Flow tool fallback |  |

## 5) Decision Log

| Decision ID | Decision | Rationale | Date | Owner |
|---|---|---|---|---|
| DEC-001 | Use Maestro Flow as primary orchestration | Best fit for linear/branching demo orchestration with agents and integrations |  |  |
