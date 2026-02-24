# Discovery And Segmentation

This partition captures the research-first workflow used before solution design.

## Purpose

- Build a strong understanding of the target operation (usually back office).
- Translate findings into 3-4 logical workflow segments.
- Decompose each segment into executable tasks mapped to automation modalities.
- Decide whether orchestration should be `BPMN` or `Case Management`.

## Files

- `use-case-research.template.md`: structured internet research capture.
- `segment-map.template.md`: segment-level architecture and handoffs.
- `task-automation-matrix.template.md`: task decomposition with technology mapping.
- `orchestration-choice.template.md`: BPMN vs case management decision matrix.

## Completion Criteria

- At least 5 quality sources summarized with citations.
- Segment map with clear entry/exit outcomes.
- Task matrix covering happy path and exception path.
- Explicit recommendation for BPMN or Case Management with rationale.
- If BPMN is recommended: reference `context/01-use-case-agentic-logic/example-bpmn.bpmn`, produce a Mermaid equivalent, and produce a valid/importable BPMN skeleton.
