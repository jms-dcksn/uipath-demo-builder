---
description: Start a local-execution Maestro Flow demo build with agents, connector/tool/control nodes, and Studio Web upload.
argument-hint: [customer, use case, or path to brief]
allowed-tools: Bash, Read, Write, Edit, Glob, Grep, WebFetch, WebSearch, AskUserQuestion, Agent
---

Start the demo-builder workflow now. The planner handles clarification before any build work.

**Input from user:** $ARGUMENTS

**Deterministic entry procedure:**

1. Invoke the `demo-builder-planner` skill via the Skill tool. If the Skill tool is unavailable, read `skills/demo-builder-planner/SKILL.md` and follow it literally.
2. Treat `$ARGUMENTS` as the user's initial input:
   - If it looks like a file path, read it and use it as the use-case brief.
   - If it names only a customer/account, run the minimum-input branch: research the account, propose 2-3 Maestro Flow demo options, and wait for selection.
   - If it describes a use case, proceed to Flow-oriented discovery.
   - If empty, ask for a customer name or use-case brief and stop until answered.
3. Follow the planner's local-execution Flow phases without skipping: preflight, discovery, Flow architecture, agents, Flow build, validation/tidy, Studio Web upload, manual checklist, and demo script.
4. Dispatch `agent-builder` only when new agent projects are required. Fan out one agent-builder instance per `AG-*` role in a single parallel turn when supported.
5. Keep all generated artifacts under `builds/<demo-slug>/`.

Begin now.
