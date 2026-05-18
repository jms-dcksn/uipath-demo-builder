---
name: demo-builder-planner
description: "Plan demo-grade UiPath Maestro Flow builds through ideation, a precise SPEC.md, a short SPEC-tightening prompt, and a structured Codex /goal prompt. Use when the user asks to design, scope, or propose a UiPath demo; provides a customer/account name for demo ideas; provides a use-case brief; or mentions Maestro Flow, AI agents, connector activities, Flow tool nodes, API Workflows, human review, Studio Web upload, or agentic orchestration. Produces planning artifacts only, not implementation."
allowed-tools: Bash, Read, Write, Edit, Glob, Grep, WebFetch, WebSearch, AskUserQuestion, Agent
---

# Demo Builder - Planner

Entry point for demo-grade UiPath Maestro Flow builds. The planner turns rough demo ideas into a precise `SPEC.md`, a short prompt to tighten that spec, and a structured `/goal` prompt for a long-running Codex build.

Only plan the demo build specification. Do not create Flow, agent, API Workflow, fixture, or solution artifacts.

## Not For

- Building demos.
- Production automation design.
- Non-local resource orchestration outside the local Flow scope.
- Running existing automations unless the user explicitly asks to debug or run them.
- Visual verification as a hard completion gate. Visual flow quality should be specified, but do not require rendered-canvas inspection in `done_when`.

## Inputs

Ideal:

- Use case title and one-paragraph business goal.
- Industry/domain.
- Known systems, connectors, tool needs, agents, documents, and tenant/folder constraints.
- Happy path and one exception path.

Minimum:

- Customer/account name. Research and propose 2-3 Flow demo options before continuing.

## Workflow

1. Ideate: turn the brief into 2-3 demo options when the scope is not already clear.
2. Interview: ask targeted questions before writing final artifacts unless the user explicitly says to skip.
3. Specify: write `SPEC.md` as the authoritative build contract.
4. Tighten: write `TIGHTEN-SPEC-PROMPT.md`, a short prompt for Codex to challenge and tighten `SPEC.md` before building.
5. Execute: write `CODEX-GOAL-PROMPT.md`, a ready-to-paste `/goal` prompt with measurable `done_when` criteria.

Research the use case when it would improve the demo story, industry accuracy, or system choices. Think in terms of digital workflow orchestration: Maestro Flow nodes, AI agents for reasoning, API Workflows for system interactions, connector/native Flow nodes, and human review where it improves the story.

Write from the perspective of the builder who will use the core UiPath skills. Do not assume unspecified Studio Web upload, tenant/folder targets, connector availability, or agent mode.

## Interview

Before writing final artifacts, ask targeted clarification questions even when the use case seems clear. Do not finalize `SPEC.md` until the user answers or explicitly authorizes proceeding with assumptions.

The interview must confirm:

- demo scope, audience, and industry context
- input/output contract
- happy path and one exception path
- systems, mock systems, connector/native Flow nodes, and API Workflow needs
- agent mode: inline Flow agents, coded agents, low-code agents, or mixed
- human review behavior
- visual Flow presentation, including layout and sticky notes
- validation expectations
- Studio Web upload expectation when the user has not specified it up front

For each question you ask - provide your best recommendation on the approach or answer to that question.

## Demo Scope

- Local-execution Maestro Flow nodes.
- AI agents as the featured reasoning component.
- Inline Flow agents, coded agents, low-code agents, or mixed agent mode, based on user choice.
- Project-backed API Workflows for system-of-record interactions, wired as nodes in the Flow.
- Connector activities, Flow tool nodes, Flow control nodes, and human review only where they support the demo story.

## Outputs

- Write `SPEC.md`.
- Write `TIGHTEN-SPEC-PROMPT.md`.
- Write `CODEX-GOAL-PROMPT.md`.
- Add supporting `demo-build-plan/` files only when the build is too large for `SPEC.md` to stay readable.

Do not write `DEMO-BUILD-PLAN.md`.

`SPEC.md` must include:

- business goal, audience, demo story, happy path, and exception path
- assumptions and explicit non-goals
- solution shape and Flow node sequence
- start input contract, output contract, and ready-to-paste sample input
- selected agent mode and agent responsibilities
- API Workflow, connector, native Flow node, and mock-system contracts
- human review task contract when applicable
- visual Flow design requirements: clean layout, readable grouping, sticky note zones, varied note colors, and no overlapping nodes
- fixtures and expected outputs
- validation checks for Flow, agents, API Workflows, fixtures, solution registration, and Studio Web upload when requested

`TIGHTEN-SPEC-PROMPT.md` must be short. It should tell Codex to read `SPEC.md`, identify ambiguous or unverifiable requirements, recommend fixes, tighten the spec in place, avoid adding scope, and stop before building.

`CODEX-GOAL-PROMPT.md` must use these blocks:

- `<goal>`
- `<context>`
- `<constraints>`
- `<done_when>`
- `<workflow>`
- `<verification_loop>`
- `<execution_rules>`
- `<output_contract>`

`done_when` must be concrete and measurable. Include Studio Web upload only when the user requested it during the interview or up front. Do not make visual canvas inspection a hard completion criterion.

Before finalizing, use `references/spec-quality-checklist.md` and `references/goal-prompt-template.md` as needed.
