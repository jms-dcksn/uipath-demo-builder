---
name: demo-builder-agents
description: "Build demo-grade UiPath AI agents for a demo (coded or low-code). Scaffolds each AG-* agent with the `uip` CLI (`uip codedagent new` / `uip agent init`), writes a design brief (role, boundaries, prompt, tools, escalation, output contract), defaults the coded path to `uipath-langchain` + `create_agent` + `ContextGroundingRetriever`, supports optional streamable HTTP MCP tools, and keeps interfaces identical between real and mock tools. Use when the demo needs one or more AI agents. Typically invoked by demo-builder-planner after orchestration and data model are defined."
allowed-tools: Bash, Read, Write, Edit, Glob, Grep, AskUserQuestion
---

# Demo Builder — Agents

Build the AI agents that power the demo. **Demo-grade** — optimize for a consistent, story-friendly happy path and one exception path, not production robustness.

## When to use

- Planner has one or more `AG-*` roles identified in the task matrix.
- User wants an agent implemented and scaffolded for a demo.

## CLI prerequisite

```bash
which uip > /dev/null 2>&1 && echo "uip found" || echo "uip NOT found — run: npm install -g @uipath/cli"
```

If missing, install via `npm install -g @uipath/cli` (ask user to install Node.js first if `npm` is missing).

## Branching questions

Ask the user up front via `AskUserQuestion`:

1. **Agent mode** — *coded* (Python, `uipath-langchain` + `create_agent`, full control) or *low-code* (`agent.json` via Agent Builder, no Python).
   - Default for this skill: **coded**. Only switch to low-code if the user asks or if the demo story is "configure without writing code."
2. **Deploy target** — *Studio Web* (agent runs as a UiPath Studio Web project) or *Orchestrator* (published package, Orchestrator-managed process).
   - Hand off Orchestrator deploy to `uipath-platform`.

## Per-agent rules (from AGENTS working rules)

- **1:1 mapping** — every `AG-*` ID gets its OWN scaffolded project (`uip codedagent new <agent-name>` for coded, `uip agent init <agent-name>` for low-code). Never multiplex roles/prompts in one runtime.
- **Design brief before code** — fill `templates/agent-build-spec.template.md` per agent (role, boundaries, prompt strategy, tool policy, escalation, output contract).
- **Context Grounding** — if the user provides an index, wire `ContextGroundingRetriever` with both `index_name` AND `folder_path`.
- **MCP** — if the user provides a streamable HTTP MCP URL for an agent, integrate MCP tools and capture the contract in the build spec.
- **Mock tools** — when a real integration is unavailable, implement a mock tool with deterministic outputs for the demo's pre-defined paths. Keep the mock interface identical to the eventual real tool so replacement is low-friction.

## Coded path (default)

1. `uip codedagent new <agent-name>` in the agent's subdirectory.
2. Use `uipath-langchain` + `create_agent`. Python SDK docs: https://uipath.github.io/uipath-python/langchain/quick_start/
3. Wire tools from the build spec — real tools where available, mock tools otherwise. Keep signatures identical.
4. If Context Grounding index provided → add `ContextGroundingRetriever(index_name=..., folder_path=...)` as a tool.
5. If MCP URL provided → register streamable HTTP MCP tools for that agent only.
6. Run `uip codedagent init` to generate `entry-points.json` / `uipath.json` / `bindings.json` after the agent code is in place.
7. Smoke-test locally with `uip codedagent run <entrypoint> '<input-json>'` against the pre-staged demo records.

For deeper coded guidance (framework choices, HITL, tracing, evaluations), defer to the installed `uipath-agents` skill's `references/coded/*`.

## Low-code path

1. Solution must exist first — `uip solution new "<SOLUTION_NAME>"` if needed.
2. Inside the solution: `uip agent init "<AGENT_NAME>"` to scaffold `agent.json` + `project.uiproj`.
3. Edit `agent.json`: system prompt, input/output schemas, tools (pre-built UiPath tools or Integration Service), Context resources (index-backed), escalations.
4. `uip agent validate "<AGENT_NAME>" --output json` after each bulk of edits. Never hand-edit `.agent-builder/` or `storageVersion`.
5. Keep the surface minimal — one happy path + one exception path.

For `agent.json` format details, defer to the installed `uipath-agents` skill's `references/lowcode/*`.

## Deploy

- **Studio Web** → publish via `uip` CLI; runtime is Studio Web project.
- **Orchestrator** → hand off to `uipath-platform` for package publish and process configuration.

## Completion criteria

- One scaffolded project per `AG-*` ID.
- `agent-build-spec.template.md` filled for each agent with real tool contracts OR clearly marked mock tools.
- Context Grounding and MCP integrations wired where the user supplied them.
- Agent runs the demo's happy path and one exception path consistently.
- Deploy path chosen and documented in the build spec.

## Templates

- `templates/agent-build-spec.template.md`

## See also

- Installed `uipath-agents` skill — production lifecycle, framework selection, evaluations, HITL.
- Installed `uipath-platform` skill — Orchestrator package/process deploy.
