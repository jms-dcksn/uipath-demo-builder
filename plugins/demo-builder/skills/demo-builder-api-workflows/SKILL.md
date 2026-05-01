---
name: demo-builder-api-workflows
description: "Build deterministic passthrough UiPath API workflow mocks for demo-builder Flow demos. Creates API-* contracts, hardcoded mock payloads, matching output schemas, passthrough workflow JSON, and local run validation."
allowed-tools: Bash, Read, Write, Edit, Glob, Grep, AskUserQuestion
---

# Demo Builder - API Workflows

Builds demo-grade mock API workflows for system-of-record interactions. The pattern is deliberately simple: a passthrough UiPath API workflow with a Response node that returns a hardcoded, realistic JSON payload.

## Scope

- In scope: deterministic mock API workflow JSON, payload contracts, output schema alignment, local run validation, and Flow handoff metadata.
- Out of scope: live API calls, credentials, real endpoints, real customer data, real patient data, real account numbers, and production integration patterns.
- Default operation: one API workflow per `API-*` contract.

## Inputs

Read these artifacts before authoring:

- `discovery/system-interactions.md`
- `discovery/mock-api-payload-research.md`
- `discovery/payload-field-map.md`
- `discovery/task-automation-matrix.md`
- `flow/flow-architecture.md`
- `flow/node-contracts.md`
- `docs/API Workflow-Workflow.json` as the passthrough Response-node reference

## Workflow

1. Confirm the intended `API-*` contracts and named systems.
2. Create `builds/<demo-slug>/apis/<API-id>/`.
3. Copy `templates/api-workflow-contract.template.md` to `apis/<API-id>/api-workflow-contract.md`.
4. Copy `templates/mock-response-payload.template.json` to `apis/<API-id>/payload.json` and replace it with the selected demo-safe payload.
5. Copy `templates/passthrough-api-workflow.template.json` to `apis/<API-id>/<WorkflowName>.json`.
6. Generate the workflow output schema from the same payload source used by `payload.json` and the Response node body.
7. Validate that every JSON path in `discovery/payload-field-map.md` for the `API-*` exists in `payload.json`.
8. Validate payload JSON and workflow JSON with a structured parser.
9. If `uip api-workflow` is unavailable, run or ask to run `uip tools install @uipath/api-workflow-tool`, then re-check `uip api-workflow --help`.
10. Run local passthrough validation:

```bash
uip api-workflow run builds/<demo-slug>/apis/<API-id>/<WorkflowName>.json --no-auth
```

11. Confirm the returned response matches `payload.json`.
12. If a project directory or `.uip` project exists for the API workflow, run:

```bash
uip api-workflow build <projectPath>
uip api-workflow pack <projectPath> <destinationPath>
```

13. Record run/build/pack results and skipped reasons in `manifest.md`.
14. Hand `API-*` registry and binding notes back to `demo-builder-flow`.

## Payload Rules

Every mock payload should include:

- Identity fields for traceability, such as `customerId`, `policyNumber`, `caseId`, `ticketId`, or `purchaseOrderId`.
- Decision fields for Flow condition logic or agent reasoning, such as `coverageStatus`, `riskScore`, `slaBreach`, `eligibilityStatus`, or `entitlementStatus`.
- Narrative fields for agent and script context, such as `lossDescription`, `recentInteractions`, `coverageSummary`, `issueSummary`, or `nextBestAction`.

Keep payloads small enough to inspect during a demo. Use anonymized values and obvious mock identifiers.

## Contract Rules

- System names must be concrete, not generic categories.
- Operation names should look like system operations, such as `getPolicyCoverage`, `lookupCustomerProfile`, `getShipmentStatus`, or `checkEntitlement`.
- Output schema and Response body must be generated from the same payload source so they cannot drift.
- Each downstream consumer must reference an explicit JSON path from `payload-field-map.md`.
- If a field is needed by an agent or condition branch, do not bury it only in prose.
- Fixture-backed Flow tool injection is a fallback only when an API workflow cannot be created, registered, or bound in time.

## Validation Gates

- Payload JSON is valid.
- Workflow JSON is valid.
- Output schema matches the payload shape.
- Every mapped downstream JSON path exists in the payload.
- `uip api-workflow run <Workflow.json> --no-auth` returns the expected mock payload, unless the API workflow tool is unavailable and the skip is recorded.
- Project-backed workflows build and pack before solution upload or packaging, when a project directory exists.
- No secrets, real personal data, real account numbers, or live endpoints appear in payloads.
- Handoff docs clearly label every API workflow as a mock passthrough resource.

## Handoff To Flow

For each `API-*`, provide:

- API workflow display name.
- Node label.
- Expected registry node type: `uipath.core.api-workflow.<resourceKey>`.
- Binding subtype: `Api`.
- Binding orchestrator type: `api`.
- Output variable shape: `$vars.<apiNodeId>.output`.
- JSON paths consumed by agents, conditions, connectors, and End outputs.
- Local validation result.
- Registry discovery status or fixture fallback.

## Templates

- `templates/api-workflow-contract.template.md`
- `templates/mock-response-payload.template.json`
- `templates/passthrough-api-workflow.template.json`
