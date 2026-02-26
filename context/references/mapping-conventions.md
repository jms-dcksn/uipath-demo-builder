# Mapping Conventions

## 1) Naming

- Use `PascalCase` for use-case names.
- Use `camelCase` for JSON field names.
- Use `UPPER_SNAKE_CASE` for fixed code values when needed.

## 2) IDs

- Case IDs: `CASE-YYYY-NNNNN`
- Requirement IDs: `BR-###`
- Segment IDs: `SEG-##`
- Rule IDs: `R-###`
- Task IDs: `T-###`
- Gateway IDs: `G-###`
- Component IDs: `CMP-<TYPE>-##`
- Agent IDs: `AG-<PURPOSE>-##`
- Demo message IDs: `MSG-##`
- Demo visual IDs: `VIS-##`

## 3) Status Values

Use this canonical status set unless there is a documented exception:

- `New`
- `InReview`
- `PendingInfo`
- `Approved`
- `Rejected`
- `Closed`

## 4) Traceability

- Every requirement (`BR-*`) must map to at least one field and one flow step.
- Every segment (`SEG-*`) must contain one or more tasks (`T-*`).
- Every UI action must map to a payload contract and business outcome.
- Every orchestration decision point should reference a rule ID (`R-*`).

## 5) Execution Type Labels

Use one of the following values consistently:

- `AI Agent`
- `RPA`
- `IDP`
- `API`
- `Human Task`

## 6) Documentation Hygiene

- Use ISO 8601 timestamps in examples.
- Mark unknowns as `TBD` with owner/date.
- Keep version numbers in each major artifact.

## 7) Agent Packaging Rules

- Keep a `1:1` mapping between each `AG-*` ID and a scaffolded UiPath agent project.
- Record the bootstrap command explicitly as `uipath new <agent-name>` for each identified agent.
- Do not implement multi-role prompt/tool multiplexing inside one agent runtime.
- Shared helper modules are allowed, but each agent must keep its own `main.py` prompt contract and task-aligned tool list.
