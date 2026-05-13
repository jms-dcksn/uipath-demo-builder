---
name: demo-builder-planner
description: "Plan demo-grade UiPath Maestro Flow builds and produce a SPEC.md. Use when the user asks to design, scope, or propose a UiPath demo; provides a customer/account name for demo ideas; provides a use-case brief; or mentions Maestro Flow, AI agents, connector activities, Flow tool nodes, Flow control nodes, API Workflows, human review, or agentic orchestration. Produces planning artifacts only, not implementation."
allowed-tools: Bash, Read, Write, Edit, Glob, Grep, WebFetch, WebSearch, AskUserQuestion, Agent
---

# Demo Builder - Planner

Entry point for demo-grade UiPath Maestro Flow builds. The planner produces a demo SPEC.md document that will be passed to a coding agent to build with the UiPath Skills.

Only plan the demo build specification. Do not create Flow, agent, API Workflow, fixture, or solution artifacts.

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
- Happy path and one exception path.

Minimum:

- Customer/account name. Research and propose 2-3 Flow demo options before continuing.

## Approach

Research the use-case with web searches to sharpen your understanding and knowledge of the industry, business domain and use-case characteristics. Think of the use-case in terms of what can be automated with digital workflows (API flows, AI Agents for cognitive tasks, - and orchestration to centrally coordinate all these components across a process function).

Create a mental model of how you would automate the process. 

Then think about how you would write a SPEC.md based on the UiPath CLI and Skills you have access to. 

Lastly, interview the user to confirm your understanding with the user, and fill any gaps in your understanding. Ask about literally anything related to automating the use-case. Your goal is to craft a tight, clear, precise SPEC.md so another AI can use the UIPath CLI and Skills to build the demo.

The scope of what will be built in the demo is as follows:
- Local-execution Maestro Flow nodes.
- AI agents as the featured reasoning component.
- Project-backed API Workflows for system-of-record interactions - these would be wired as nodes in the Flow.
- Connector activities, Flow tool nodes, Flow control nodes, and human review only where they support the demo story.

## Outputs

- Write `SPEC.md`.
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
