# Frontend Demo Design

This partition defines the UI architecture and page-level experience for demo delivery.

## Inputs

- `context/02-case-entity-model/case-entity.schema.template.json`
- `context/00-discovery-and-segmentation/task-automation-matrix.template.md`
- Orchestration design from `context/01-use-case-agentic-logic/`

## Files

- `frontend-architecture.template.md`: app structure, data flow, and integration model.
- `dashboard-page.template.md`: multi-page dashboard, full-width header, collapsible sidebar, and case/process list interaction design.
- `case-detail-page.template.md`: instance detail page for case/process context, variables, metadata, and decision actions.
- `ui-data-contract.template.md`: exact mapping from UI elements to entity/task data.

## Completion Criteria

- App is explicitly defined as a multi-page experience.
- Dashboard and instance detail pages are clearly specified.
- Dashboard shell includes a full-width header bar and collapsible sidebar with use-case-relevant menu placeholders.
- Clicking a case row or process instance row navigates to a separate detail route/page.
- User task actions and case updates are mapped to backend operations.
- Every displayed field has a source and fallback behavior.
- Placeholder methods are documented for future Data Fabric case-entity integration after entity creation is confirmed.
- Demo storytelling cues are embedded in page-level design notes.
- UI contracts include both case-entity and task-level data.
- Page notes identify visuals that can be referenced in the final demo script (`VIS-*`).
