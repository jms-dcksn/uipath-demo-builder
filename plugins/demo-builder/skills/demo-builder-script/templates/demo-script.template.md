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
| VIS-01 | Flow topology | Studio Web Flow canvas | Trigger, agents, placeholders or native API nodes, connectors, tools, and control nodes are orchestrated in one Flow |
| VIS-02 | Agent output | Flow run output or logs | AI result drives routing |
| VIS-03 | API Workflow project | Studio Web solution project list or API project folder | Mock system context exists as an uploaded `Api` project |
| VIS-04 | API invocation placeholder | Flow script node or native API node | Current Flow invocation mode matches the documented API output contract |

## 4) Message-To-Proof Alignment

Each message must map to 2-3 proof points.

| Message ID | Proof IDs | Why This Pairing Works |
|---|---|---|
| MSG-01 | VIS-01, VIS-02 |  |

## 5) Run Of Show

| Beat # | Message ID | Proof ID | Narration | Operator Action | Visible Outcome |
|---|---|---|---|---|---|
| 1 | MSG-01 | VIS-01 |  | Open Flow | Audience sees trigger, agent, connector/tool/control, and End nodes |

## 6) Opening And Close

- Opening value statement:
- Closing business impact statement:

## 7) Contingency Lines

| Risk During Live Demo | Fallback Line | Fallback Action |
|---|---|---|
| Connector unavailable | "The Flow contract is already wired; this fixture shows the same output shape while the tenant connection is refreshed." | Open fixture output |
| API Workflow node not bindable yet | "The API Workflow projects are in the solution; this Flow uses equivalent script-backed placeholders until native inline binding is available." | Open the API project and the placeholder node contract |

## 8) Rehearsal Checklist

- Flow validates after tidy.
- Operator actions were dry-run in order.
- Any debug/run action has user approval.
- API Workflow projects are uploaded as `projectType: Api`.
- Flow invocation mode is documented as native API node, script placeholder, or not used.
- Timing stays within target duration.
- Script language matches actual build behavior.
