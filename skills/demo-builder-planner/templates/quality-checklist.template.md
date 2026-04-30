# Demo Builder Quality Checklist

Use this after editing the skill set or before publishing the plugin.

## 1) Scope Consistency

| Check | Pass/Fail | Notes |
|---|---|---|
| Skills describe Maestro Flow as the default orchestration model |  |  |
| Skills emphasize local-execution node families: agents, connectors, Flow tools, and Flow control |  |  |
| Active skills do not require deferred products |  | Search stale references |
| Flow project is solution-contained |  |  |
| Manual-trigger demo inputs are treated as a visible operator contract |  |  |
| Sub-agent path references match plugin layout |  |  |

## 2) Build Reliability

| Component | Criteria | Pass/Fail | Notes |
|---|---|---|---|
| Planner | Manifest, phase order, resource blockers, checklist |  |  |
| Discovery | Source register, process map, Flow task matrix, checkpoint |  |  |
| Flow | Registry discovery, operator input surface, node contracts, connector bindings, validate/tidy |  |  |
| Agents | One project per `AG-*`, Flow contract, validation status |  |  |
| Script | Only actual Flow proof points, rehearsal checklist, fallback lines |  |  |

## 3) Consistency Search

```bash
rg -n '<stale product or workflow term>' README.md skills agents commands plugins/demo-builder
rg -n '<deprecated command or companion skill>' README.md skills agents commands plugins/demo-builder
```
