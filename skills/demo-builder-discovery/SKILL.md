---
name: demo-builder-discovery
description: "Interactive discovery and segmentation for UiPath Case Management demo builds. Clarifies ambiguous requests with the user, conducts internet research on the target operation, incorporates any reference docs the user drops into a project-root `docs/` folder, decomposes the operation into 3-4 segments, and builds a task-to-automation-type matrix (AI Agent / RPA / IDP / API / Human Task). Use at the start of any demo build. Typically invoked by demo-builder-planner."
allowed-tools: Bash, Read, Write, Edit, Glob, Grep, WebFetch, WebSearch, AskUserQuestion
---

# Demo Builder — Discovery

Interactive, research-first phase for demo builds. Turns a use case + industry into segments and a task matrix that feeds downstream Case Management design. **Case Management is the only orchestration model** for this skill set.

## When to use

- Planner has a use case and needs operational research.
- User wants to validate segmentation for a demo.

## Scope disclosure — tell the user up front

Before going deep, state what the demo-builder skills will and will not do:

- **Will build:** discovery artifacts, Data Fabric case entity JSON, Case Management design + `sdd.md` handed to the production `uipath-case-management` skill for `caseplan.json` generation, AI agents (coded or low-code scaffolds), Vite + React + TypeScript frontend, a narrated demo script.
- **Will NOT build / deploy:** production RPA/IDP/API workflows (specs only — handed off), tenant-side Data Fabric imports, live Orchestrator deployment (delegated to `uipath-platform`), anything requiring credentials the user has not provided.
- **Demo-grade only:** mock tools with deterministic outputs are preferred over half-working live integrations. Happy path + one exception path.

Make this clear in the first reply so the user can course-correct before you invest research time.

## Interactive guidance — ask, don't assume

Before starting research, use `AskUserQuestion` to resolve anything ambiguous. Treat these as a checklist. If the user answered all of them already in their prompt, skip — otherwise ask:

1. **Use case framing** — confirm the one-line business goal and the operation name (e.g., "KYC onboarding for US commercial banking").
2. **Industry / sub-industry** — generic vs regulated/regional variant changes research scope meaningfully.
3. **Timebox / demo duration** — default 6-10 minutes; affects how many segments and agents are realistic.
4. **Known systems** — any specific APIs, document types, or systems of record the demo must feature? (If none, you'll pick credible stand-ins.)
5. **Must-have vs nice-to-have** — any specific features (e.g., "must show document IDP", "must include an MCP tool") the user has already committed to.
6. **Exception path** — is there a particular exception they want highlighted (fraud hit, policy violation, missing doc), or pick one during segmentation?

If the user gives a vague prompt like *"build me a demo for banking"*, you MUST ask clarifying questions before researching. Do not guess.

## Reference documentation intake

Reference docs the user provides (SOPs, policy PDFs, sample forms, API docs, screenshots) dramatically improve research quality. Set this up early, but do not block indefinitely if the user declines or asks for fast presales mode:

1. Check for a `docs/` directory in the project root (`ls docs/` at repo root). Create it if missing:
   ```bash
   mkdir -p docs
   ```
2. Tell the user:
   > I've created a `docs/` folder in the project root. Drop any reference material there — SOPs, policy docs, regulatory references, sample forms, API documentation, screenshots of existing tools. I'll read everything in that folder before finalizing research. Let me know when you're done adding files.
3. Wait for the user to confirm they've added files, explicitly say "no docs to add", or choose fast presales mode.
4. Inventory the folder: `ls docs/` then `Read` each file. Cite user-provided docs as `[DOC-###]` (separate ID space from `[SRC-###]` web sources).
5. User-provided docs OUTRANK web sources when there is conflict — flag conflicts explicitly to the user.

## Workflow

1. **Scope + clarify** — state scope disclosure, ask clarifying questions via `AskUserQuestion`, set up `docs/`, and confirm whether the user wants full research or fast presales mode.
2. **Research the operation** using `WebSearch` + `WebFetch` PLUS `docs/` contents. Follow `references/research-and-citation-rules.md`: ≥5 sources for full mode; ≥3 sources for fast presales mode. Full mode still needs ≥2 sources describing real operational workflows and ≥1 on compliance/risk where applicable. Tag web claims with `[SRC-###]` and doc claims with `[DOC-###]`. Copy `templates/use-case-research.template.md` to `builds/<demo-slug>/discovery/use-case-research.md` and fill it.
3. **Register sources** — copy `templates/source-register.template.md` to `builds/<demo-slug>/discovery/source-register.md` and record every `[SRC-###]` and `[DOC-###]`.
4. **Segment** the operation into 3-4 logical workflow segments with clear entry/exit outcomes that can map cleanly onto Case Management stages. Copy `templates/segment-map.template.md` to `builds/<demo-slug>/discovery/segment-map.md` and fill it. Each segment gets a `SEG-##` ID.
5. **Decompose** each segment into tasks (`T-###`) with execution type: `AI Agent`, `RPA`, `IDP`, `API`, `Human Task`. Cover happy path AND the user-confirmed exception path. Copy `templates/task-automation-matrix.template.md` to `builds/<demo-slug>/discovery/task-automation-matrix.md` and fill it.
6. **Checkpoint with the user** — show the segment map and task matrix and ask via `AskUserQuestion` whether to proceed to Case Management design. Do not hand off silently.

## Completion criteria

- Scope disclosure delivered to the user.
- All ambiguity resolved via `AskUserQuestion` before research started.
- `docs/` folder created; user-provided reference material inventoried (or user explicitly declined).
- Source register written; full mode has ≥5 quality sources; fast presales mode has ≥3 quality sources; user-provided docs cited as `[DOC-###]`.
- Segment map with clear entry/exit outcomes that fit a Case Management lifecycle.
- Task matrix covering happy path + one exception path.
- User approved the segment map + task matrix before hand-off.

## Hand-off

- Back to planner, which invokes `demo-builder-data-fabric` (case entity) and `demo-builder-case-management` (stage/task design → sdd.md → production `uipath-case-management`).

## References

- `references/research-and-citation-rules.md`
- `references/mapping-conventions.md` — IDs and execution-type labels

## Templates

- `templates/use-case-research.template.md`
- `templates/source-register.template.md`
- `templates/segment-map.template.md`
- `templates/task-automation-matrix.template.md`
