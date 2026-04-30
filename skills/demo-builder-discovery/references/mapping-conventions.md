# Mapping Conventions

Discovery produces IDs that downstream Flow architecture must preserve.

| Prefix | Meaning |
|---|---|
| `BR-*` | Business requirement |
| `SEG-*` | Process segment |
| `T-*` | Task |
| `AG-*` | Agent candidate |
| `CONN-*` | Connector activity candidate |
| `TOOL-*` | Flow tool node candidate |
| `CTRL-*` | Flow control node candidate |
| `PRE-*` | Prerequisite or blocker |

## Execution Types

Use exactly one execution type per task:

- `AI Agent`
- `Connector Activity`
- `Flow Tool`
- `Flow Control`
- `Trigger`

Every `AG-*`, `CONN-*`, `TOOL-*`, and `CTRL-*` must include enough naming detail for registry discovery.
