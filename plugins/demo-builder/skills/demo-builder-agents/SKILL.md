---
name: demo-builder-agents
description: "Build demo-grade UiPath AI agents used by a Maestro Flow. Supports coded agents with uipath-langchain, low-code agent.json projects, and inline Flow agents. Keeps every AG-* mapped to a Flow node contract."
allowed-tools: Bash, Read, Write, Edit, Glob, Grep, AskUserQuestion
---

# Demo Builder - Agents

Build or document the AI agents invoked by the Flow. Keep each agent small, role-specific, and aligned to its Flow node contract.

Agents are the one primary non-control component in this demo-builder. Use them for reasoning, classification, extraction, summarization, drafting, and judgment. Keep deterministic enrichment and routing in connector, Flow tool, or Flow control nodes.

## When To Use

- Flow architecture identifies one or more `AG-*` roles.
- User wants a coded, low-code, or inline agent for a demo.
- Planner needs an agent build spec before Flow wiring.

## Branching Questions

Ask up front when not already specified:

1. Agent source: existing published agent, existing in-solution sibling agent, new standalone coded agent, new standalone low-code agent, or new inline Flow agent.
2. For new standalone agents: coded or low-code.
3. For coded agents: LangChain default unless the user asks for another supported framework.
4. Runtime target: local sibling in solution or inline Flow agent by default; individual agent push/deploy only when the user explicitly asks. The planner uploads the full solution to Studio Web.

## Rules

- One `AG-*` maps to one agent project or one inline agent definition.
- Do not multiplex multiple role prompts inside one runtime.
- Coded path defaults to `uipath-langchain` and `create_agent`.
- Use `uv` for Python package management.
- If Context Grounding is provided, include both `index_name` and `folder_path`.
- If an MCP URL is provided, integrate streamable HTTP MCP tools for that agent only.
- Keep real and mock tool interfaces identical.
- Agent output contracts must be shaped for downstream Flow node wiring.
- Do not use an agent to hide work that should be visible as connector, Flow tool, or Flow control nodes in the demo.

## Coded Path

1. Read the installed `uipath-agents` coded quickstart.
2. Copy `templates/agent-build-spec.template.md` to `builds/<demo-slug>/agents/<AG-id>/agent-build-spec.md`.
3. Run setup in the agent directory:
   ```bash
   uv add uipath-langchain
   uv sync
   uip codedagent setup --output json
   uip codedagent new <agent-name>
   ```
4. Implement `main.py` with `uipath-langchain` and `create_agent`.
5. Wire only the tools needed for this agent's Flow node contract.
6. Run `uip codedagent init` after code changes.
7. Create `evaluations/eval-sets/smoke-test.json`.
8. Smoke-test locally when the CLI/auth state allows it.

## Low-Code Path

1. Use `uip agent init <agent-name>` for standalone low-code agents.
2. Edit `agent.json` for prompt, model, input schema, output schema, tools, context, and escalation.
3. Validate with `uip agent validate <agent-project> --output json`.
4. Keep the output contract simple enough for Flow consumption.

## Inline Agent Path

1. Use the Flow project directory:
   ```bash
   uip agent init <FlowProjectDir> --inline-in-flow --output json
   ```
2. Record the returned project ID.
3. Configure the inline `agent.json`.
4. Validate:
   ```bash
   uip agent validate <FlowProjectDir>/<projectId> --inline-in-flow --output json
   ```
5. Hand the project ID to `demo-builder-flow` for the `uipath.agent.autonomous` node.

## Completion Criteria

- One build spec exists per `AG-*`.
- Every built agent has a clear Flow node input and output contract.
- Existing agents have registry discovery notes instead of duplicate scaffolds.
- New coded agents have a smoke eval artifact.
- Validation/local run status is recorded with skipped reasons when not run.

## Templates

- `templates/agent-build-spec.template.md`
