# Task Automation Matrix Template

Map each segment to Flow-buildable tasks and resources.

## 1) Task Matrix

| Segment ID | Task ID | Task Name | Purpose | Execution Type | Deliverable Artifact | Component ID | Input | Output | Exception Path |
|---|---|---|---|---|---|---|---|---|---|
| SEG-01 | T-001 | Normalize trigger payload | Convert inbound payload to Flow context | Flow Tool | Flow script/transform node | TOOL-001 | Trigger payload | Normalized context | Return validation error |
| SEG-01 | T-002 | Lookup account entitlement | Return deterministic mock system context | Mock API Workflow | Studio Web API Workflow project + script placeholder invoking equivalent mock payload | API-001 | Account ID | Entitlement payload | Use documented placeholder output |
| SEG-01 | T-003 | Classify request | Reason over context and entitlement payload | AI Agent | Flow agent node | AG-001 | Normalized context + `API-001` output | Classification + reason | Route to review |
| SEG-02 | T-004 | Shape result | Prepare downstream payload | Flow Tool | Flow script/transform node | TOOL-002 | Classification + API payload | Enriched context | Use exception fixture |
| SEG-03 | T-005 | Notify adjuster | Send status update | Connector Activity | Integration Service connector activity | CONN-001 | Message payload | Send confirmation | Log connector error |

## 2) Task Dependencies

| Task ID | Depends On | Dependency Type |
|---|---|---|
| T-002 | T-001 | Data prerequisite |
| T-003 | T-002 | Mock system context |

## 3) Execution Type Guidance

- `AI Agent` when reasoning, summarization, classification, or judgment is needed.
- `Mock API Workflow` when the demo needs deterministic system-of-record context from a named system. Use `API-*` component IDs and always build a Studio Web API Workflow project.
- `Connector Activity` when UiPath Integration Service has a curated connector activity.
- `Flow Tool` when deterministic local work is enough, such as script parsing, payload shaping, static transforms, or fixture-backed enrichment.
- `Flow Control` when routing, branching, looping, merging, delay, subflow, end, or termination is needed.
- `Trigger` for the Flow entry event.

## 4) Deliverable Artifact Guidance

- API workflow artifact: `Studio Web API Workflow project`.
- Native Flow invocation when available: `Flow API Workflow node`.
- Current placeholder invocation: `Script placeholder invoking equivalent mock payload`.
- Do not describe a script placeholder as the API Workflow artifact. It is only the Flow invocation stand-in until native API Workflow nodes can call the API Workflow project inline.

## 5) Task-Level Demo Targets

| Task ID | Demo Proof | Latency Target | Audit/Trace Need |
|---|---|---|---|
| T-002 | Mock system payload visible in Flow context | 10 sec | Capture payload field paths |
| T-003 | Agent output visible in Flow result | 30 sec | Capture reason |
