---
name: demo-builder-case-management
description: "Design a demo-grade UiPath Case Management flow from research + task matrix: map segments to stages, tasks from the automation matrix to case tasks (with skeleton tasks + mock agents where integrations are unavailable), produce a Mermaid stage diagram for user review, then synthesize a minimal sdd.md for architect-owned delegation to `uipath-case-management`. Use when planner has chosen Case Management as the orchestration model. Typically invoked by demo-builder-planner."
allowed-tools: Bash, Read, Write, Edit, Glob, Grep, AskUserQuestion
---

# Demo Builder — Case Management

Turn discovery output (segments + task matrix) into a **demo-grade** Case Management design and produce the `sdd.md` handoff for the architect. **This skill does not generate `caseplan.json` itself** — the architect invokes the production `uipath-case-management` skill from the planner thread.

## When to use

- `demo-builder-discovery` recommended Case Management.
- User wants a Case Management flow designed and built for a demo.

## Inputs

- Segment map (`SEG-*`) from `demo-builder-discovery`
- Task automation matrix (`T-*` with execution types) from `demo-builder-discovery`
- Case entity schema from `demo-builder-data-fabric`

## Mapping rules

- **Segment → Stage.** One `SEG-##` usually becomes one case stage. Split a segment into multiple stages only when evidence/approval gates change the lifecycle state.
- **Task → Case Task.** Every `T-###` becomes a task on a stage. Preserve the execution-type label (`AI Agent`, `RPA`, `IDP`, `API`, `Human Task`, `Trigger`, `Intermediate Event`).
- **Human Tasks** → Action tasks with a small, opinionated form (match fields to the case entity).
- **AI Agents** → one case task per `AG-*`. 1:1 mapping to a scaffolded agent project. Agents that consume upstream stub output must reference the stub ID in their design brief.
- **Everything else (`RPA` / `IDP` / `API` / `Trigger` / `Intermediate Event`)** → **stub contract** authored here at design time, per `references/stub-contract-rules.md`. The production skill receives these as skeleton tasks in `sdd.md` and will insert `<UNRESOLVED: ...>` markers for task-type-ids and connection-ids — do not fabricate those.
- **Entity alignment** — every stub output field that persists must map to a field in the Data Fabric case entity schema. Case lifecycle/state field must match the stages and their transitions.

## Workflow

1. Copy `templates/case-management-design.template.md` to `builds/<demo-slug>/flow-model/case-management-design.md` and fill it end-to-end:
   - §1 Case type
   - §2 **Trigger stub contract** (exactly one, with hardcoded demo payloads)
   - §3 Stages (3-5) with entry/exit criteria
   - §4 Tasks per stage, with execution type
   - §5 **Non-agent stub contracts** — one row per `RPA`/`API`/`IDP` task with inputs, outputs, hardcoded demo values for happy + exception paths, mock hint, real-build note. See `references/stub-contract-rules.md`.
   - §6 Intermediate events (only if the flow waits mid-stage)
   - §7 Transitions (including the one exception-path edge)
   - §8 Mermaid diagram: agents solid, stubs dashed, trigger + events distinct
   - §9 Handoff summary — bullet list of every `CMP-*` the user must build for real later
2. Present the **Mermaid stage diagram + the handoff summary + the stub I/O table** to the user via `AskUserQuestion` for review. Do NOT proceed until the user approves. The stub I/O is the contract the frontend and agents will code against — catching it wrong here is expensive.
3. After approval, copy `templates/sdd.template.md` to `builds/<demo-slug>/flow-model/sdd.md` and synthesize a **minimal `sdd.md`** for the installed `uipath-case-management` skill:
   - Trigger (from §2) — declared I/O; the Trigger node is auto-created by the production skill
   - Stages + edges (from §3, §7)
   - Tasks per stage with type + display name (from §4). For each stub task, include declared inputs/outputs from §5 so the production skill can wire them where resolvable; leave `task-type-id` and `connection-id` unresolved.
   - Any task-entry conditions for the exception path
4. Return `sdd.md` to the architect. The architect (planner thread) owns delegation to `uipath-case-management`. Do not invoke that skill from inside this skill.
5. **Record manual deploy assumptions** — include Case Management publish/deploy items and any unresolved skeleton tasks in the handoff summary so the architect can add them to `builds/<demo-slug>/manual-completion-checklist.md` after `caseplan.json` generation.

## Demo rules

- Keep stages to **3-5**. If research pushed you to 6+, merge adjacent stages.
- Prefer **stubs with hardcoded I/O + mock agents** over half-built real integrations. The demo story depends on consistent outputs, not live systems.
- **Identical interfaces** for mock and real. A stub's output shape must match what the real component will eventually produce, so swap is friction-free.
- One happy path + one exception path. Nothing more.
- Pre-stage a small varied set of case records (normal / urgent / exception) — align with `demo-builder-data-fabric` example records.

## Completion criteria

- `case-management-design.template.md` filled end-to-end, including every stub contract in §2, §5, §6.
- Every non-agent / non-human task has a stub row with concrete I/O schema and hardcoded demo values for happy + exception.
- Stub output fields that persist map to case entity fields.
- Mermaid stage diagram + handoff summary reviewed and approved by the user.
- `sdd.md` written and handed back to the architect for delegation.
- Manual completion checklist inputs identified for skeleton-task and publish/deploy follow-up.

## Templates

- `templates/case-management-design.template.md`
- `templates/sdd.template.md`

## References

- `references/stub-contract-rules.md` — canonical non-agent stub format (RPA / API / IDP / Trigger / Intermediate Event)

## See also

- Installed `uipath-case-management` skill — owns `sdd.md` → `tasks.md` → `caseplan.json` generation, skeleton task semantics, registry discovery, validation. The architect invokes it from the planner thread; do not duplicate its 24 critical rules here.
- Installed `uipath-platform` skill — Studio Web solution publish.
