# Process Map Template

Define the top-level business flow as 3-4 logical segments that can be represented in Maestro Flow.

## 1) Segment Overview

| Segment ID | Segment Name | Business Goal | Entry Trigger | Exit Outcome |
|---|---|---|---|---|
| SEG-01 | Intake | Capture complete request context | Trigger payload received | Context normalized |

## 2) Segment Handoffs

| From Segment | To Segment | Handoff Artifact | Handoff Condition |
|---|---|---|---|
| SEG-01 | SEG-02 | Normalized context | Required fields complete |

## 3) Segment Risks

| Segment ID | Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|---|
| SEG-02 | Missing connector connection | Medium | High | Use fixture or document prerequisite |

## 4) Segment KPI Mapping

| Segment ID | KPI | Baseline | Target |
|---|---|---|---|
| SEG-03 | Time to decision | 2 days | 1 hour |

## 5) Flow Storyline

- Segment to highlight first:
- Segment with strongest agent story:
- Segment with strongest mock API/connector/tool/control story:
- Segment with exception branch:
