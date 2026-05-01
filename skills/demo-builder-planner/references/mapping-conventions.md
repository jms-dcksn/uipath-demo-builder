# Mapping Conventions

Use stable IDs across artifacts.

| Prefix | Meaning |
|---|---|
| `BR-*` | Business requirement |
| `SEG-*` | Business process segment |
| `T-*` | Task in the automation matrix |
| `FLOW-*` | Flow-native node or step |
| `API-*` | Mock API workflow |
| `AG-*` | Agent |
| `CONN-*` | Connector activity |
| `TOOL-*` | Flow tool node |
| `CTRL-*` | Flow control node |
| `PRE-*` | Prerequisite or blocker |
| `MSG-*` | Demo message |
| `VIS-*` | Demo proof point or visual |

## Execution Types

- `AI Agent`
- `Mock API Workflow`
- `Connector Activity`
- `Flow Tool`
- `Flow Control`
- `Trigger`

Every task must map to at least one local-execution Flow node, deterministic mock API workflow, or explicit blocker. Live external API calls, RPA workflows, Human Tasks, Case Management, Data Fabric, Coded Apps, frontends, queues, and other resource-invocation nodes are outside the default scope.
