# Agent Tool Discovery

Use this reference when a demo includes any `AG-*` component, especially low-code or inline Flow agents. Capture the tool and knowledge plan before Flow architecture so build work does not stall on tenant resources that must be prepared manually.

## Per-Agent Discovery Questions

For every `AG-*`, capture:

- Agent role and mode: existing published, existing local sibling, coded, low-code, or inline Flow agent.
- Context Grounding choice: not used, existing index, or new index required.
- Additional tool preference: UiPath GenAI Activity web search, Integration Service connector activity, MCP, deterministic mock/stub, or none.
- Tool readiness: ready in tenant, needs manual setup, intentionally mocked, or blocked.

## Context Grounding Readiness

If Context Grounding is selected, record both `index_name` and `folder_path` before agent build or Flow wiring.

For an existing index, capture:

- Index name.
- Folder path and tenant/folder scope.
- Retriever tool name, when the agent configuration needs one.
- User confirmation that the index exists and contains the expected content.
- Any expected document topics or policy/SOP coverage the agent should cite or reason from.

For a new index, capture:

- Source document set the user will provide.
- Target folder path.
- Desired index name.
- Manual creation owner and status.
- Whether the demo should proceed with a deterministic mock/stub until the index exists.

Do not assume the `uip` CLI can create a Context Grounding index programmatically. Treat new-index creation as a tenant-side prerequisite unless current CLI behavior proves otherwise during the build.

## Additional Tool Selection

Prefer one clear additional tool per agent unless the user explicitly asks for more.

- Use UiPath GenAI Activity web search when the agent needs public or current information and repeatability risk is acceptable.
- Use an Integration Service connector activity when the demo needs a visible live system read/write with an existing connection.
- Use MCP when the user provides a specific MCP endpoint and tool names.
- Use deterministic mock/stub tools for repeatable presales walkthroughs or when live resources are not ready.
- Use no extra tool when the agent only needs Flow inputs, mock API payloads, and optional Context Grounding.

## Blocking Rules

Block or checkpoint before architecture when:

- A low-code or inline agent needs Context Grounding but `index_name` or `folder_path` is unknown.
- The user wants a new Context Grounding index but has not provided the source document set or agreed to manual tenant setup.
- A connector-backed tool is material to the demo but connection or folder details are unknown.
- The requested tool would require live external API calls outside the default local-execution scope.

Document unresolved setup in `handoff/manual-completion-checklist.md` instead of guessing resource names, folder paths, connection IDs, or index names.
