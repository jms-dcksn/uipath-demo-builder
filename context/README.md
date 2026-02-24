# Context Harness Overview

This folder contains partitioned context documents for designing and delivering UiPath demos end-to-end.

## Workflow Partitions

1. `00-discovery-and-segmentation/`: internet research, operation understanding, segment and task decomposition, orchestration choice.
2. `01-use-case-agentic-logic/`: convert discovery outputs into model-ready process/case logic.
3. `02-case-entity-model/`: define Data Fabric case entity schemas and requirement traceability.
4. `03-frontend-demo-design/`: define dashboard and case-detail UX with strict data contracts.
5. `04-implementation-specs/`: detailed component specs, agent build specs, backlog, and acceptance scenarios.
6. `references/`: shared standards for naming, quality, and citation rules.

## Expected Output Artifacts

- Research-backed operating model summary with sources.
- Segment and task matrix mapped to `AI Agent`, `RPA`, `IDP`, `API`, and `Human Task`.
- Recommended orchestration pattern (`BPMN` or `Case Management`) with rationale.
- Model-ready orchestration design packet.
- If `BPMN` is selected: comparison against `context/01-use-case-agentic-logic/example-bpmn.bpmn`, a Mermaid flow representation, and a valid/importable BPMN skeleton for downstream wiring.
- Case entity schema/example JSON for Data Fabric import.
- Frontend architecture and page contracts.
- Implementation and handoff pack for proprietary components plus buildable assets for agents/frontend.
- Suggested demo script with `3-4` key messages, each aligned to `2-3` concrete visuals and operator actions.

## Authoring Rules

- Keep IDs stable across partitions (`BR-*`, `SEG-*`, `T-*`, `R-*`).
- Maintain end-to-end traceability from requirement to task to data to UI.
- Mark unknowns as `TBD` with owner/date.
- Keep citations for all material research claims.
