# Demo Builder Quality Checklist

Use this after editing the skill set or before publishing the plugin.

## 1) Scope Consistency

| Check | Pass/Fail | Notes |
|---|---|---|
| Skills describe Case Management as the only orchestration model |  | Search for stale procedural-orchestration language |
| No skill tells the agent to write into source `templates/` |  | Filled artifacts belong under `builds/<demo-slug>/` |
| Sub-agent path references match plugin layout (`agents/`) |  |  |

## 2) Build Reliability

| Component | A-Grade Criteria | Pass/Fail | Notes |
|---|---|---|---|
| Planner | Manifest, phase order, reconciliation loop, manual checklist |  |  |
| Discovery | Source register, segment map, task matrix, user checkpoint |  |  |
| Data Fabric | Schema, 3 fixtures, reconciliation notes, JSON validation |  |  |
| Case Management | Case design, `sdd.md`, user approval, `tasks.md`, `caseplan.json` |  |  |
| Agents | One project per `AG-*`, current `uip codedagent` lifecycle, smoke eval |  |  |
| Frontend | UiPath Coded Web App, SDK contract, local run, `npm run build` |  |  |
| Script | Only implemented `VIS-*`, rehearsal checklist, fallback lines |  |  |

## 3) Consistency Search

Run these before publishing, replacing placeholders with the stale terms you are checking:

```bash
rg -n '<stale-scope-or-command-terms>' README.md skills agents commands
rg -n '<source-template-write-target-terms>' skills agents commands
```
