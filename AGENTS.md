# UiPath Demo Builder Context Harness

This repository is a context harness for building UiPath demos from a use case, industry, and requirements notes.
It is optimized for Maestro-centric solutions with Data Fabric persistence and custom front ends.

## Minimum Inputs From User

- Use case title and one-paragraph business goal.
- Industry and domain.
- Demo requirements and constraints.
- Known systems/APIs/documents (if available).

## Standard Delivery Workflow

1. Research the operation with targeted internet sources.
2. Decompose work into 3-4 logical segments.
3. Break each segment into executable tasks.
4. Map each task to execution type: `AI Agent`, `RPA`, `IDP`, `API`, `Human Task`.
5. Choose orchestration model: `BPMN` for procedural/autonomous, or `Case Management` for adaptive/human-heavy.
6. Define case data model in Data Fabric (if case-based).
7. Produce implementation specs for components and UI.
8. Build what is supported directly (agents, frontend, case entity JSON) and hand off proprietary components with detailed specs.
9. Produce a suggested demo script with `3-4` key messages, each mapped to `2-3` concrete demo visuals.

## Capability Contract


| Area                  | Can Do                                                                          | Cannot Do                                                        |
| --------------------- | ------------------------------------------------------------------------------- | ---------------------------------------------------------------- |
| Use-case discovery    | Perform web research, summarize operations, extract process patterns            | Guarantee full domain completeness without SME review            |
| Process modeling      | Produce BPMN-ready logic and case management stage/task diagrams in Mermaid     | Directly edit proprietary case modeling editors                  |
| Automation components | Write detailed RPA/API/IDP specifications and contracts                         | Build proprietary RPA/IDP workflows inside locked tooling        |
| AI agents             | Build Python agents with UiPath SDK, prefer `uipath-langchain` + `create_agent` | Bypass tenant/runtime constraints outside available environment  |
| Data Fabric           | Produce case entity schema/example JSON for import                              | Execute tenant-side imports without platform access              |
| Frontend              | Build Vite + React + TypeScript app using UiPath TypeScript SDK                 | Validate against inaccessible tenant configs without credentials |


## Reference Docs

- UiPath LangChain quick start:
  - `https://uipath.github.io/uipath-python/langchain/quick_start/`
- UiPath TypeScript SDK getting started:
  - `https://uipath.github.io/uipath-typescript/getting-started/`

## Context Map

- Overview and workflow:
  - `context/README.md`
- Discovery and decomposition:
  - `context/00-discovery-and-segmentation/README.md`
  - `context/00-discovery-and-segmentation/use-case-research.template.md`
  - `context/00-discovery-and-segmentation/segment-map.template.md`
  - `context/00-discovery-and-segmentation/task-automation-matrix.template.md`
  - `context/00-discovery-and-segmentation/orchestration-choice.template.md`
- Use case and orchestration logic:
  - `context/01-use-case-agentic-logic/README.md`
  - `context/01-use-case-agentic-logic/use-case-brief.template.md`
  - `context/01-use-case-agentic-logic/industry-domain-analysis.template.md`
  - `context/01-use-case-agentic-logic/business-logic-layer.template.md`
  - `context/01-use-case-agentic-logic/maestro-design.template.md`
  - `context/01-use-case-agentic-logic/case-management-design.template.md`
- Case entity modeling:
  - `context/02-case-entity-model/README.md`
  - `context/02-case-entity-model/requirements-to-case-entity.template.md`
  - `context/02-case-entity-model/case-entity.schema.template.json`
  - `context/02-case-entity-model/case-entity.example.json`
  - `context/02-case-entity-model/data-fabric-modeling-notes.template.md`
- Frontend demo design:
  - `context/03-frontend-demo-design/README.md`
  - `context/03-frontend-demo-design/frontend-architecture.template.md`
  - `context/03-frontend-demo-design/dashboard-page.template.md`
  - `context/03-frontend-demo-design/case-detail-page.template.md`
  - `context/03-frontend-demo-design/ui-data-contract.template.md`
- Implementation specs and handoff:
  - `context/04-implementation-specs/README.md`
  - `context/04-implementation-specs/component-specifications.template.md`
  - `context/04-implementation-specs/agent-build-spec.template.md`
  - `context/04-implementation-specs/demo-script.template.md`
  - `context/04-implementation-specs/delivery-backlog.template.md`
  - `context/04-implementation-specs/acceptance-scenarios.template.md`
- Shared references:
  - `context/references/README.md`
  - `context/references/DemoScriptReference.docx`
  - `context/references/demo-script-authoring-notes.md`
  - `context/references/mapping-conventions.md`
  - `context/references/demo-design-principles.md`
  - `context/references/research-and-citation-rules.md`

## Example Packs

- Filled KYC example:
  - `context/examples/kyc-us-commercial-banking/README.md`

## Working Agreement

- Treat templates as working source artifacts.
- Keep one use case per branch or per dedicated folder copy.
- Maintain requirement-to-task-to-data traceability across all partitions.
- Document deviations from template defaults in the corresponding README.

## Demo Approach and Considerations

- Any code you write is used for demos only - avoid defensive programming, excessive try/catches - KEEP IT SIMPLE
- The code just needs to work consistently for a small set of pre-defined paths with consistent data - we're just telling stories, not writing production code.

## Demo Script Deliverable Rules

- Keep to `3-4` key messages only.
- Map every key message to `2-3` visuals with explicit page/system names.
- For each script beat, provide: narrator line, operator action, and on-screen proof/outcome.
- Include one opening value statement and one closing business-impact statement.
- Keep script aligned with the implemented artifacts (orchestration, agent outputs, and frontend screens).

## Success Criteria

- A very clear path to creating the Maestro (BPMN or Case Management) flow is provided to the user
- Clear descriptions on all proprietary workflows (API, RPA, IDP) that need to be built provided to the user
- You have provided documents to include for the IDP extraction service that will be used
- AI Agents (uipath-langchain) are fully built and tested
- Front end experience matches requirements and you have given the user clear instructions on setting .env variables to test locally.
- You have provided a suggested demo script that emphasizes 3-4 key messages, with alignment to 2-3 demo visuals for each key message.
