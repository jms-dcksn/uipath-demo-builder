# Task Automation Matrix Template

Map each segment to Flow-buildable tasks and resources.

## 1) Task Matrix

| Segment ID | Task ID | Task Name | Purpose | Execution Type | Component ID | Input | Output | Exception Path |
|---|---|---|---|---|---|---|---|---|
| SEG-01 | T-001 | Normalize trigger payload | Convert inbound payload to Flow context | Flow Tool | TOOL-001 | Trigger payload | Normalized context | Return validation error |
| SEG-01 | T-002 | Classify request | Reason over context | AI Agent | AG-001 | Normalized context | Classification + reason | Route to review |
| SEG-02 | T-003 | Enrich context | Add demo-safe fixture or computed details | Flow Tool | TOOL-002 | Classification | Enriched context | Use exception fixture |
| SEG-03 | T-004 | Notify adjuster | Send status update | Connector Activity | CONN-001 | Message payload | Send confirmation | Log connector error |

## 2) Task Dependencies

| Task ID | Depends On | Dependency Type |
|---|---|---|
| T-002 | T-001 | Data prerequisite |

## 3) Execution Type Guidance

- `AI Agent` when reasoning, summarization, classification, or judgment is needed.
- `Connector Activity` when UiPath Integration Service has a curated connector activity.
- `Flow Tool` when deterministic local work is enough, such as script parsing, payload shaping, static transforms, or fixture-backed enrichment.
- `Flow Control` when routing, branching, looping, merging, delay, subflow, end, or termination is needed.
- `Trigger` for the Flow entry event.

## 4) Task-Level Demo Targets

| Task ID | Demo Proof | Latency Target | Audit/Trace Need |
|---|---|---|---|
| T-002 | Agent output visible in Flow result | 30 sec | Capture reason |
