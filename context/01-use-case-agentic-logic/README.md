# Use Case To Agentic Logic

This partition converts business context into orchestration-ready logic for Maestro BPMN and/or Case Management.

## Inputs

- `context/00-discovery-and-segmentation/use-case-research.template.md`
- `context/00-discovery-and-segmentation/segment-map.template.md`
- `context/00-discovery-and-segmentation/task-automation-matrix.template.md`
- `context/00-discovery-and-segmentation/orchestration-choice.template.md`

## Files

- `use-case-brief.template.md`: core business framing and success criteria.
- `industry-domain-analysis.template.md`: domain boundaries, roles, events, and constraints.
- `business-logic-layer.template.md`: decision and orchestration rules for the agentic layer.
- `maestro-design.template.md`: BPMN-oriented workflow design template.
- `case-management-design.template.md`: case-lifecycle-oriented design template.
- `example-bpmn.bpmn`: BPMN reference artifact to compare structure/patterns for demo-quality process design.

## Completion Criteria

- Business goals and success metrics are explicit and measurable.
- Segment decomposition and task mappings are represented in model logic.
- Roles, events, and exception paths are fully captured.
- Handoffs between automation and humans are unambiguous.
- BPMN/case lifecycle artifacts can be built directly from this partition.
- If orchestration is `BPMN`, the design references `example-bpmn.bpmn` as a comparison baseline.
- If orchestration is `BPMN`, both a Mermaid flow and a valid importable BPMN skeleton are produced.
