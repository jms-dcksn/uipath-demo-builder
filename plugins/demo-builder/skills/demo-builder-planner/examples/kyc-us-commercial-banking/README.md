# KYC Example Pack - US Commercial Banking Onboarding

This is a fully filled example of the demo-builder context harness for a KYC onboarding use case.

## Scenario

A mid-size US commercial bank wants to reduce KYC onboarding cycle time for new business checking accounts while improving compliance coverage.

## Demo Objective

Show how UiPath Maestro can orchestrate AI Agents, IDP, API checks, and human review across a case lifecycle, with Data Fabric as the persistent case store and a React frontend for operations users.

## Chosen Orchestration Pattern

- Primary recommendation: `Case Management`
- Reason: KYC onboarding has frequent exceptions, rework loops, and discretionary human decisions.

## Folder Map

- `00-discovery-and-segmentation/`: research, source register, process segmentation, and task automation matrix.
- `01-use-case-agentic-logic/`: use-case and Case Management logic.
- `02-case-entity-model/`: Data Fabric case entity mapping and JSON artifacts.
- `03-frontend-demo-design/`: multi-page dashboard and instance detail UX contracts.
- `04-implementation-specs/`: component specs, agent build spec, demo script, backlog, acceptance scenarios.
- `references/`: supporting citation register for this example.

## Notes

- Regulatory references are included for demo design context and do not replace legal advice.
- Validate jurisdiction-specific requirements with compliance stakeholders before production build.
- This example is Case-Management-first and does not include a procedural orchestration variant.
