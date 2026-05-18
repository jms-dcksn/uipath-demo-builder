# Spec Quality Checklist

Use this before finalizing `SPEC.md`.

- The spec is demo-grade and avoids production hardening unless required for the demo to run.
- The user has chosen agent mode: inline Flow agents, coded agents, low-code agents, or mixed.
- Studio Web upload is either explicitly requested or explicitly out of scope.
- The spec defines business goal, audience, demo story, happy path, exception path, and non-goals.
- The Flow section defines solution shape, node sequence, routing, start input contract, output contract, and sample manual-start input.
- Agent responsibilities, inputs, output schema, and examples are clear.
- API Workflows, connectors, native Flow nodes, and mock systems have concrete contracts.
- Human review is included only when it improves judgment, approval, missing-information, or data-quality handling.
- Visual Flow design is specified: clean layout, readable groups, sticky note zones, varied note colors, and no overlapping nodes.
- Fixtures include at least one happy path and one exception path with expected final outputs.
- Validation checks are measurable and do not depend on subjective visual inspection as a hard completion gate.
- No requirement depends on an unverified capability unless it is labeled as an assumption or blocker.
