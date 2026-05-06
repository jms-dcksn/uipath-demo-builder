# Task Automation Matrix Template

Map each segment to Flow-buildable tasks and resources.

## 1) Task Matrix

| Segment ID | Task ID | Task Name | Purpose | Execution Type | Deliverable Artifact | Component ID | Input | Output | Agent Tools / Knowledge Source | Readiness / Blocker | Exception Path |
|---|---|---|---|---|---|---|---|---|---|---|---|
| SEG-01 | T-001 | Normalize trigger payload | Convert inbound payload to Flow context | Flow Tool | Flow script/transform node | TOOL-001 | Trigger payload | Normalized context | N/A | Ready | Return validation error |
| SEG-01 | T-002 | Lookup account entitlement | Return deterministic mock system context | Mock API Workflow | Studio Web API Workflow project + script placeholder invoking equivalent mock payload | API-001 | Account ID | Entitlement payload | N/A | Payload research pending | Use documented placeholder output |
| SEG-01 | T-003 | Classify request | Reason over context and entitlement payload | AI Agent | Flow agent node | AG-001 | Normalized context + `API-001` output | Classification + reason | Context Grounding existing index or not used; optional web search/connector/MCP/mock | Pending user preference | Route to review |
| SEG-02 | T-004 | Shape result | Prepare downstream payload | Flow Tool | Flow script/transform node | TOOL-002 | Classification + API payload | Enriched context | N/A | Ready | Use exception fixture |
| SEG-03 | T-005 | Notify adjuster | Send status update | Connector Activity | Integration Service connector activity | CONN-001 | Message payload | Send confirmation | N/A | Connection pending discovery | Log connector error |

## 2) Task Dependencies

| Task ID | Depends On | Dependency Type |
|---|---|---|
| T-002 | T-001 | Data prerequisite |
| T-003 | T-002 | Mock system context |

## 3) Execution Type Guidance

- `AI Agent` when reasoning, summarization, classification, or judgment is needed.
- For every `AI Agent`, document its knowledge source and tools: Context Grounding status, index/folder readiness, web search, connector activity, MCP, deterministic mock/stub, or none.
- `Mock API Workflow` when the demo needs deterministic system-of-record context from a named system. Use `API-*` component IDs and always build a Studio Web API Workflow project.
- `Connector Activity` when the task is a visible live external-system read/write and UiPath Integration Service has a curated connector activity with a usable connection.
- `Flow Tool` when deterministic local work is enough, such as script parsing, payload shaping, static transforms, or fixture-backed enrichment.
- `Flow Control` when routing, branching, looping, merging, delay, subflow, end, or termination is needed.
- `Trigger` for the Flow entry event.

## 4) Deliverable Artifact Guidance

- API workflow artifact: `Studio Web API Workflow project`.
- Native Flow invocation when available: `Flow API Workflow node`.
- Current placeholder invocation: `Script placeholder invoking equivalent mock payload`.
- Do not describe a script placeholder as the API Workflow artifact. It is only the Flow invocation stand-in until native API Workflow nodes can call the API Workflow project inline.
- Connector artifact: `Integration Service connector activity`.
- Do not use a connector activity as deterministic mock system context. Use `Mock API Workflow` for repeatable demo data and `Connector Activity` for live Integration Service actions.

## 5) Connector Activity Details

| Connector ID | Service | Connector Key | Activity Intent | Required Connection / Folder | User-Visible Output | Blocker Status |
|---|---|---|---|---|---|---|
| CONN-001 |  |  | Send status update | Existing enabled connection in target folder | Send confirmation | Pending discovery |

## 6) Agent Tool Details

| Agent ID | Agent Mode | Context Grounding Choice | Index Name | Folder Path | Source Documents / Topics | Additional Tool | Tool Readiness | Manual Setup Needed |
|---|---|---|---|---|---|---|---|---|
| AG-001 | coded / low-code / inline / existing | none / existing index / new index required |  |  |  | GenAI Activity web search / connector / MCP / mock / none | Pending | Create index or select existing index if needed |

## 7) Task-Level Demo Targets

| Task ID | Demo Proof | Latency Target | Audit/Trace Need |
|---|---|---|---|
| T-002 | Mock system payload visible in Flow context | 10 sec | Capture payload field paths |
| T-003 | Agent output visible in Flow result | 30 sec | Capture reason |
| T-005 | Connector action visible in Flow output or target system | 10 sec | Capture connection and operation metadata |
