# Delivery Backlog - KYC Onboarding Demo

## 1) Milestone Plan

| Milestone | Goal | Exit Criteria | Target Date |
|---|---|---|---|
| M1 | Discovery complete | Research and decomposition approved | 2026-02-24 |
| M2 | Model complete | Case lifecycle and task contracts approved | 2026-02-27 |
| M3 | Build assets complete | Entity JSON, agent package, frontend baseline complete | 2026-03-06 |
| M4 | Demo ready | Acceptance scenarios passed for critical paths and script dry run complete | 2026-03-10 |

## 2) Work Items

| Item ID | Type | Description | Depends On | Owner | Status |
|---|---|---|---|---|---|
| WI-001 | Modeling | Configure case stages and task templates | M1 | Automation Lead | Planned |
| WI-002 | Data | Import `KycOnboardingCase` schema and seed sample cases | WI-001 | Data Lead | Planned |
| WI-003 | AI Agent | Implement AG-COMP-01 and AG-BRIEF-01 | WI-001 | AI Lead | Planned |
| WI-004 | Integration | Implement sanctions and registry API connectors | WI-001 | Integration Lead | Planned |
| WI-005 | Frontend | Build multi-page dashboard and instance detail pages with SDK | WI-002 | Frontend Lead | Planned |
| WI-006 | RPA | Build legacy core update bot | WI-001 | RPA Lead | Planned |
| WI-007 | Demo Script | Create run-of-show with key-message-to-visual mapping and operator cues | WI-003, WI-005 | Demo Architect | Planned |
| WI-008 | Validation | Run end-to-end acceptance scenarios and scripted dry run | WI-003, WI-004, WI-005, WI-006, WI-007 | QA Lead | Planned |

## 3) Ownership Model

- Product/demo owner: Solutions Consultant
- Process SME: Compliance Manager
- Automation lead: UiPath Architect
- Frontend lead: Full-stack Engineer
- Data model owner: Data Fabric Engineer

## 4) Risks And Mitigations

| Risk | Probability | Impact | Mitigation | Owner |
|---|---|---|---|---|
| Screening API sandbox unavailable | Medium | High | Mock adapter with contract-compatible responses | Integration Lead |
| Reviewer policy disagreements | Medium | Medium | Lock policy workshop before build sprint | Compliance SME |
| Legacy UI instability for RPA | High | Medium | Selector hardening + fallback manual step for demo | RPA Lead |

## 5) Decision Log

| Decision ID | Decision | Rationale | Date | Owner |
|---|---|---|---|---|
| DEC-001 | Use Case Management as primary model | Exception-heavy, human-judgment workflow | 2026-02-21 | Demo Architect |
| DEC-002 | Keep importable BPMN skeleton as secondary artifact | Supports procedural variant discussion and downstream web-editor wiring | 2026-02-21 | Demo Architect |
