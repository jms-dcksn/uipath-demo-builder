# Plan Quality Checklist

Use this checklist before finalizing `DEMO-BUILD-PLAN.md` and the `demo-build-plan/` detail files.

- The plan stays demo-grade and does not introduce production hardening unless required for the demo to run.
- The root index includes assumptions, file map, build order, and non-negotiable constraints.
- The demo vision includes business goal, research grounding, demo story, happy path, exception path, and why the demo works.
- The Flow architecture includes solution shape, node sequence, start input contract, output contract, and routing behavior.
- The plan includes a complete sample manual-start input.
- The agent section defines agent purpose, inputs, output schema, happy-path example, and exception-path example.
- Human review is included when the exception path needs judgment, approval, missing information, or coverage/data-quality control.
- API Workflow interactions are represented as real project-backed API Workflow deliverables when they make system-of-record interactions credible.
- Script fallback is allowed only when direct API Workflow binding is unavailable; it must not replace the API Workflow projects in the solution.
- Fixtures include at least one happy path and one exception path with expected final outputs.
- Validation includes Flow validation, API Workflow build checks, solution registration, happy-path result, and exception-path result.
- The handoff tells the builder to ask the user whether to upload the completed solution to Studio Web.
