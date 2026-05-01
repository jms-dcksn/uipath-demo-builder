# Support Ticket Flow Demo Example

Small Flow-first example pack used to show the intended artifact shape.

## Demo Slice

- Trigger: manual trigger with a support ticket payload.
- Mock API workflow: `API-001` returns a deterministic ServiceNow entitlement payload.
- Agent: `AG-001` classifies urgency and recommends routing.
- Flow tool: `TOOL-001` normalizes the ticket payload.
- Connector: `CONN-001` posts a team notification.
- Control: `CTRL-001` routes high urgency or inactive entitlement to an exception result.
- End output: returns `demoResult`.

## Files

- `discovery/task-automation-matrix.md`
- `discovery/system-interactions.md`
- `discovery/mock-api-payload-research.md`
- `discovery/payload-field-map.md`
- `flow/flow-architecture.md`
- `flow/node-contracts.md`
- `apis/API-001/api-workflow-contract.md`
- `apis/API-001/payload.json`
- `apis/API-001/ServiceNowEntitlementLookup.json`
- `agents/AG-001/agent-build-spec.md`
- `script/demo-script.md`
