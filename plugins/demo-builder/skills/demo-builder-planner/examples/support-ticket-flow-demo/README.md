# Support Ticket Flow Demo Example

Small Flow-first example pack used to show the intended artifact shape.

## Demo Slice

- Trigger: manual trigger with a support ticket payload.
- Agent: `AG-001` classifies urgency and recommends routing.
- Flow tool: `TOOL-001` normalizes the ticket payload and `TOOL-002` enriches it with demo-safe fixture details.
- Connector: `CONN-001` posts a team notification.
- Control: `CTRL-001` routes high urgency to an exception result.
- End output: returns `demoResult`.

## Files

- `discovery/task-automation-matrix.md`
- `flow/flow-architecture.md`
- `flow/node-contracts.md`
- `agents/AG-001/agent-build-spec.md`
- `script/demo-script.md`
