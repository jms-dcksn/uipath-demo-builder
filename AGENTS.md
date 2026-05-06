# UiPath Demo Builder Project Notes

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
