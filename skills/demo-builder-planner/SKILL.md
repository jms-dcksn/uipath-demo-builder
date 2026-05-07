---
name: demo-builder-planner
description: "Plan demo-grade UiPath Maestro Flow builds. Use when the user asks to design, scope, or propose a UiPath demo; provides a customer/account name for demo ideas; provides a use-case brief; or mentions Maestro Flow, AI agents, connector activities, Flow tool nodes, Flow control nodes, API Workflows, human review, or agentic orchestration. Produces planning artifacts only, not implementation."
allowed-tools: Bash, Read, Write, Edit, Glob, Grep, WebFetch, WebSearch, AskUserQuestion, Agent
---

# Demo Builder - Planner

Entry point for demo-grade UiPath Maestro Flow builds. The planner produces a demo plan document that will be passed to a coding agent to build with the UiPath Skills.

Only plan the demo build. Do not create Flow, agent, API Workflow, fixture, or solution artifacts.

## Not For

- Building demos.
- Production automation design.
- Non-local resource orchestration outside the local Flow scope.
- Running existing automations unless the user explicitly asks to debug or run them.

## Inputs

Ideal:

- Use case title and one-paragraph business goal.
- Industry/domain.
- Known systems, connectors, tool needs, agents, documents, and tenant/folder constraints.
- Demo duration.
- Happy path and one exception path.

Minimum:

- Customer/account name. Research and propose 2-3 Flow demo options before continuing.

## Approach

Default plan shape:

- Local-execution Maestro Flow nodes.
- AI agents as the featured reasoning component.
- Project-backed API Workflows for system-of-record interactions - these would be wired as nodes in the Flow.
- Connector activities, Flow tool nodes, Flow control nodes, and human review only where they support the demo story.

Use public web research when the user provides only an account, industry, or lightly specified use case. If the user provides a complete use case, use research only to ground domain assumptions and avoid expanding the scope.

Research:

- The customer
- The customer industry
- The use-case specific to that industry

After the workflow is understood, break it into segments that can be automated with APIs/integration, AI agents, conditional logic, routing, and human review. Use this breakdown to formulate the Maestro Flow demo plan.

Include one happy path and one exception path unless the user explicitly asks for a narrower scope.

Before finalizing the plan, apply `references/plan-quality-checklist.md`.

## Outputs

- Write a concise root index at `DEMO-BUILD-PLAN.md`.
- Put detailed sections in `demo-build-plan/` as separate Markdown files so the build agent can discover them incrementally.
- The root index must include assumptions, file map, build order, and non-negotiable build constraints.
- Split detail files logically, using this default structure unless the use case requires a better split:
  - `01-demo-vision.md`: business goal, research grounding, demo story, happy path, exception path, why the demo works.
  - `02-flow-architecture.md`: solution shape, Flow node sequence, start input contract, output contract, routing behavior.
  - `03-agent-and-human-review.md`: AI agent purpose, inputs, output schema, examples, human review task contract.
  - `04-api-workflows.md`: required API Workflow projects, inputs/outputs, connector mapping, delivery constraints.
  - `05-fixtures-and-validation.md`: fixture files, expected results, validation checklist.

The plan must include:

- A sample input that allows the user to manually test the Flow.
- A handoff note instructing the builder to ask whether to upload the completed solution to Studio Web.
- Validation commands or checks for the Flow, API Workflow projects, fixtures, and solution registration.
