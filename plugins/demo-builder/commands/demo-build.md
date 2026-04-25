---
description: Start a UiPath Agentic Orchestration demo build — runs preflight → discovery → data model → case management → agents → coded web app frontend → manual checklist → demo script.
argument-hint: [customer or use case — e.g. "Acme Bank KYC" or a path to a brief]
allowed-tools: Bash, Read, Write, Edit, Glob, Grep, WebFetch, WebSearch, AskUserQuestion, Agent
---

Start the demo-builder workflow now. Do not ask clarifying questions before invoking the skill — the planner handles elicitation.

**Input from user:** $ARGUMENTS

**Deterministic entry procedure — follow in order:**

1. Invoke the `demo-builder-planner` skill via the Skill tool. If the Skill tool is unavailable, read `skills/demo-builder-planner/SKILL.md` and follow it literally.
2. Treat `$ARGUMENTS` as the user's initial input:
   - If it looks like a file path, read it and use it as the use-case brief.
   - If it names a customer/account only, run the minimum-input branch (research the account, propose 2-3 use cases via `AskUserQuestion`, wait for selection).
   - If it describes a use case, proceed straight to Discovery.
   - If empty, ask the user for a customer name or use-case brief via `AskUserQuestion` — nothing else.
3. Follow the planner's delivery workflow phases without skipping. Write all artifacts under `builds/<demo-slug>/` per the planner's build directory convention.
4. Dispatch sub-agents (`agent-builder`, `frontend-builder`, `case-designer`, `data-modeler`) as the planner prescribes — `agent-builder` must be fanned out one-per-`AG-*` role in a single parallel tool call.

Begin now.
