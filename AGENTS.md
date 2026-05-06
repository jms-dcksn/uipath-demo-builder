# UiPath Demo Builder Project Notes

<!-- LOCAL_FILE_LINK_METHODOLOGY_START -->
## Authoritative Local File-Link Methodology

This section supersedes any conflicting local-file link guidance elsewhere in this file.

- For generated artifacts and important local files, use Markdown file-card links only with forward-slash absolute Windows targets, for example `[label](<C:/Users/name/projects/file.docx>)`.
- Keep the normal copyable Windows path with backslashes visible immediately below or nearby as the fallback/source of truth.
- Do not use raw backslash Windows paths inside Markdown link targets such as `[label](<C:\Users\...>)`; this chat surface can strip backslashes and create broken `C:Users...` file-card targets.
- Encoded-space forward-slash targets also work, but unencoded spaces in forward-slash targets are acceptable when the target is wrapped in angle brackets.
- PowerPoint file-card preview may hang on left-click even when Open works; validate PPTX links by the Open action or the visible path.
- Keep Markdown links for web/source-system URLs normally.
<!-- LOCAL_FILE_LINK_METHODOLOGY_END -->

## Visible Message Timestamps

- Visible agent-session messages: prefix every user-visible assistant message in agent session surfaces with a concrete local date/timestamp, including progress updates, status reports, and final handoffs. Use the format `[YYYY-MM-DD HH:mm:ss -07:00]` with the active local UTC offset, and refresh the timestamp from the local clock when feasible rather than reusing stale context. Do not add timestamps to hidden reasoning, raw tool output, code blocks, or generated file contents unless the user explicitly asks for that content to be timestamped.

This checkout is a local child project based on
`https://github.com/jms-dcksn/uipath-demo-builder`.

## Project Type

- Treat this repo as a Codex plugin marketplace, not as a runnable web app.
- The Codex plugin entry is `.agents/plugins/marketplace.json`.
- The installable plugin lives at `plugins/demo-builder/`.
- The canonical skill sources are duplicated at both `skills/` and
  `plugins/demo-builder/skills/`; keep them in sync if editing skill content.

## Generated Demo Builds

- Generated customer or use-case demos belong under `builds/<demo-slug>/`.
- `builds/` and `docs/` are intentionally ignored by git in this upstream repo.
- Do not move large generated artifacts into git. If a build produces bulky data,
  store it under the shared OneDrive project data area and add a small pointer
  manifest in git if the artifact must be referenced.

## Local Preflight

Before running a real demo-builder workflow, verify:

- `uip --version`
- `uip login status --output json`
- `uip maestro case --help`
- `uip codedagent --help`
- `uip solution --help`

If those UiPath CLI surfaces are absent, stop at preflight and report the missing
surface instead of producing partial downstream artifacts.

Current local status as of 2026-04-30:

- `@uipath/cli` is installed globally at version `0.9.1`.
- `uip codedagent setup --force` completed successfully.
- Installed UiPath tools: `solution-tool`, `codedagent-tool`, `codedapp-tool`,
  and `maestro-tool`, all at version `0.9.1`.
- `uip login status --output json` reports `Not logged in`; a tenant login is
  still required before publish, deploy, or tenant-backed validation.

## Codex Plugin Enablement

Preferred local enablement from this checkout:

```bash
codex plugin marketplace add .
```

If the packaged Codex CLI is blocked by Windows execution policy or app package
permissions, use the Codex plugin marketplace UI to add this local repo, or run
the CLI from an environment where `codex.exe` can execute.
## Agent skills

### Issue tracker

Issues and PRDs are tracked in GitHub Issues using the `gh` CLI. See `docs/agents/issue-tracker.md`.

### Triage labels

The default five-label vocabulary is used: `needs-triage`, `needs-info`, `ready-for-agent`, `ready-for-human`, and `wontfix`. See `docs/agents/triage-labels.md`.

### Domain docs

This repo uses the single-context layout. See `docs/agents/domain.md`.
