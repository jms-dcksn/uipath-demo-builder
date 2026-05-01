# Task Automation Matrix Template

Map each segment to Flow-buildable tasks and resources.

## 1) Task Matrix

| Segment ID | Task ID | Task Name | Purpose | Execution Type | Component ID | Input | Output | Exception Path |
|---|---|---|---|---|---|---|---|---|
| SEG-01 | T-001 | Normalize trigger payload | Convert inbound payload to Flow context | Flow Tool | TOOL-001 | Trigger payload | Normalized context | Return validation error |
| SEG-01 | T-002 | Lookup account entitlement | Return deterministic mock system context | Mock API Workflow | API-001 | Account ID | Entitlement payload | Use fixture-backed fallback |
| SEG-01 | T-003 | Classify request | Reason over context and entitlement payload | AI Agent | AG-001 | Normalized context + `API-001` output | Classification + reason | Route to review |
| SEG-02 | T-004 | Shape result | Prepare downstream payload | Flow Tool | TOOL-002 | Classification + API payload | Enriched context | Use exception fixture |
| SEG-03 | T-005 | Notify adjuster | Send status update | Connector Activity | CONN-001 | Message payload | Send confirmation | Log connector error |

## 2) Task Dependencies

| Task ID | Depends On | Dependency Type |
|---|---|---|
| T-002 | T-001 | Data prerequisite |
| T-003 | T-002 | Mock system context |

## 3) Execution Type Guidance

- `AI Agent` when reasoning, summarization, classification, or judgment is needed.
- `Mock API Workflow` when the demo needs deterministic system-of-record context from a named system. Use `API-*` component IDs.
- `Connector Activity` when UiPath Integration Service has a curated connector activity.
- `Flow Tool` when deterministic local work is enough, such as script parsing, payload shaping, static transforms, or fixture-backed enrichment.
- `Flow Control` when routing, branching, looping, merging, delay, subflow, end, or termination is needed.
- `Trigger` for the Flow entry event.

## 4) Task-Level Demo Targets

| Task ID | Demo Proof | Latency Target | Audit/Trace Need |
|---|---|---|---|
| T-002 | Mock system payload visible in Flow context | 10 sec | Capture payload field paths |
| T-003 | Agent output visible in Flow result | 30 sec | Capture reason |
