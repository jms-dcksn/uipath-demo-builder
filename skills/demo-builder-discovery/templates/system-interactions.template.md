# System Interactions

Catalog likely system-of-record interactions for the demo slice.

## 1) Interaction Catalog

| Interaction ID | Segment ID | Named System | Operation Name | Business Purpose | Candidate Component ID | Mock Needed? | Notes |
|---|---|---|---|---|---|---|---|
| SYS-001 | SEG-01 | ServiceNow | `checkEntitlement` | Confirm support entitlement before triage | API-001 | Yes | Use deterministic passthrough mock |

## 2) System Selection Rationale

| Candidate System | Selected? | Reason | Material Choice? | User Checkpoint Needed? |
|---|---|---|---|---|
| ServiceNow | Yes | Common support entitlement and case context system | Yes / No | Yes / No |

## 3) Demo Boundary

- Real integration required:
- Mock passthrough acceptable:
- Fixture-backed fallback:
- Known tenant/resource prerequisites:

## 4) Downstream Consumers

| API ID | Payload Consumers | Required Fields | Demo Proof |
|---|---|---|---|
| API-001 | AG-001, CTRL-001, FLOW-END-01 | entitlement status, support level, summary | Agent uses entitlement; branch uses status |
