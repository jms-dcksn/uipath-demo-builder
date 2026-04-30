# Demo Design Principles

## 1) Start With Business Value

- Frame each Flow around a decision or outcome.
- Prioritize what proves value quickly in a live demo.

## 2) Show Realistic Workflows

- Include both happy path and exception path.
- Make agent reasoning visible and surround it with deterministic connector, Flow tool, and Flow control nodes.
- Make execution type visible where it supports the narrative: `AI Agent`, `Connector Activity`, `Flow Tool`, `Flow Control`, or `Trigger`.

## 3) Keep Data Credible

- Use realistic but anonymized fixture payloads.
- Keep fields coherent across trigger input, node contracts, agent outputs, connector payloads, tool outputs, control branches, and End output.

## 4) Make Interactions Obvious

- Flow topology should answer: "What happens next?"
- Run output should answer: "What decision did the automation make?"

## 5) Optimize For Storytelling

- Pre-stage a small set of varied examples: normal, urgent, and exception.
- Make branch outcomes visible after operator actions.

## 6) Build For Reusability

- Reuse shared conventions and template sections.
- Capture assumptions, blockers, and tenant prerequisites as you design.

## 7) Script The Demo Explicitly

- Define 3-4 key messages tied to business outcomes.
- Map each key message to 2-3 proof points.
- For every beat, write narrator line, operator action, and visible outcome.
- Progress the story from trigger to AI/integration work to routing and final output.
- End with measurable impact language, not feature recap.
