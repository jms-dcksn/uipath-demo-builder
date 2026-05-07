# AGENTS.md

## Purpose
This repository provides a minimalist **demo-builder planner skill** for presales work.

Use this skill to help sales engineers turn a customer name, use case, or short brief into a robust demo plan that can be handed off to Codex for implementation.

## Intent and Positioning
- Treat this repo as a **planning layer**, not a build system.
- The planner creates implementation-ready Markdown artifacts only.
- Codex plus the core UiPath skills remain responsible for building, validating, uploading, and debugging UiPath artifacts.
- Keep the planner focused on demo storytelling, scope control, architecture, handoff clarity, and repeatability.

## What to Optimize For
- Fast path from customer use case -> buildable demo plan -> Codex handoff.
- Demo-grade scope: clear happy path plus one exception path.
- Clear assumptions, inputs, outputs, Flow architecture, agent roles, API Workflow contracts, fixtures, and validation checks.
- Outputs that are practical for a coding agent to implement without re-discovering the use case.

## Working Expectations
- Keep this repo as simple as possible: one planner skill and its direct reference material.
- Do not reintroduce companion build skills, plugin wrappers, slash commands, or marketplace packaging unless explicitly requested.
- Do not create Flow, agent, API Workflow, fixture, solution, or Studio Web upload artifacts from the planner skill.
- The planner output should be `DEMO-BUILD-PLAN.md` plus detailed files under `demo-build-plan/`.
- Include a handoff note telling the builder to ask whether to upload the completed solution to Studio Web.
- Keep changes concise, practical, and aligned to the existing repository conventions.
- When in doubt, preserve simplicity and plan quality over build automation.
