# Mapping Conventions

Discovery produces IDs that downstream Flow architecture must preserve.

| Prefix | Meaning |
|---|---|
| `BR-*` | Business requirement |
| `SEG-*` | Process segment |
| `T-*` | Task |
| `API-*` | Project-backed mock API Workflow plus current Flow invocation mode |
| `AG-*` | Agent candidate |
| `CONN-*` | Connector activity candidate |
| `TOOL-*` | Flow tool node candidate |
| `CTRL-*` | Flow control node candidate |
| `PRE-*` | Prerequisite or blocker |

## Execution Types

Use exactly one execution type per task:

- `AI Agent`
- `Mock API Workflow`
- `Connector Activity`
- `Flow Tool`
- `Flow Control`
- `Trigger`

Every `API-*`, `AG-*`, `CONN-*`, `TOOL-*`, and `CTRL-*` must include enough naming detail for registry discovery.
