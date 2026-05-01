# Demo Script Template

Use this to produce the final narrated run-of-show for the Flow demo.

## 1) Demo Metadata

- Use case:
- Version:
- Author:
- Date:
- Target audience:
- Duration target:

## 2) Key Messages

| Message ID | Key Message | Business Proof Point | Related Requirement IDs |
|---|---|---|---|
| MSG-01 |  |  | BR-001 |

## 3) Proof Inventory

| Visual/Proof ID | Name | Location | What It Proves |
|---|---|---|---|
| VIS-01 | Flow topology | Studio Web Flow canvas | Mock API workflows, agents, connectors, tools, and control nodes are orchestrated in one Flow |
| VIS-02 | Agent output | Flow run output or logs | AI result drives routing |
| VIS-03 | Mock API payload | API workflow output or node contract | System context uses documented demo-safe fields |

## 4) Message-To-Proof Alignment

Each message must map to 2-3 proof points.

| Message ID | Proof IDs | Why This Pairing Works |
|---|---|---|
| MSG-01 | VIS-01, VIS-02 |  |

## 5) Run Of Show

| Beat # | Message ID | Proof ID | Narration | Operator Action | Visible Outcome |
|---|---|---|---|---|---|
| 1 | MSG-01 | VIS-01 |  | Open Flow | Audience sees trigger, API workflow, agent, connector, tool, control, and End nodes |

## 6) Opening And Close

- Opening value statement:
- Closing business impact statement:

## 7) Contingency Lines

| Risk During Live Demo | Fallback Line | Fallback Action |
|---|---|---|
| Connector unavailable | "The Flow contract is already wired; this fixture shows the same output shape while the tenant connection is refreshed." | Open fixture output |
| API workflow not bindable | "The mock API contract and payload are stable; this fixture-backed node preserves the same output shape for the demo." | Open payload field map |

## 8) Rehearsal Checklist

- Flow validates after tidy.
- Operator actions were dry-run in order.
- Any debug/run action has user approval.
- Mock API workflow local run completed or fallback is documented.
- Timing stays within target duration.
- Script language matches actual build behavior.
