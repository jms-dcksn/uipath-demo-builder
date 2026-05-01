---
name: demo-builder-api-workflows
description: "Build project-backed deterministic passthrough UiPath API Workflow mocks for demo-builder Flow demos. Creates API-* contracts, hardcoded mock payloads, matching output schemas, Studio Web API project folders, solution registration, build validation, and upload handoff evidence."
allowed-tools: Bash, Read, Write, Edit, Glob, Grep, AskUserQuestion
---

# Demo Builder - API Workflows

Builds demo-grade mock API workflows for system-of-record interactions. The pattern is deliberately simple: a project-backed UiPath API Workflow with a Response node that returns a hardcoded, realistic JSON payload.

## Scope

- In scope: deterministic mock API workflow JSON, payload contracts, output schema alignment, project-backed `Type: Api` folders, solution registration, local run/build validation, and Flow handoff metadata.
- Out of scope: live API calls, credentials, real endpoints, real customer data, real patient data, real account numbers, and production integration patterns.
- Default operation: one API Workflow project per `API-*` contract.
- Flow invocation is tracked separately from API Workflow artifact delivery. Until native inline Flow API Workflow binding is available, use script-backed placeholders in the Flow with the same output contract; do not skip the API Workflow projects.

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
12. When a solution exists, create `<SolutionDir>/<ApiProjectName>/` and write:
    - `Workflow.json` from the same passthrough workflow JSON, with `document.tags.projectId` and `document.tags.solutionId` set when known.
    - `project.uiproj` with `"ProjectType": "Api"` and `"MainFile": "Workflow.json"`.
    - `entry-points.json` with one entry point of `type: "Api"`, `filePath: "content/Workflow.json"`, a stable `uniqueId`, and an output schema generated from `payload.json`.
    - `bindings_v2.json`, using an empty resources array for pure passthrough mocks.
    - `.local/ProjectSettings.json` with empty breakpoint/watch/activity metadata.
13. Build the API Workflow project:

```bash
uip api-workflow build <SolutionDir>/<ApiProjectName> --output json
```

14. Register the project in the solution manifest:

```bash
uip solution project add <SolutionDir>/<ApiProjectName> <SolutionDir>/<SolutionName>.uipx --output json
```

15. Inspect the `.uipx` and confirm one `Type: "Api"` entry exists for the project.
16. Record JSON/local run/build/solution registration results and skipped reasons in `manifest.md`.
17. Hand `API-*` project evidence and Flow invocation status back to `demo-builder-flow`.

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
- Script-backed Flow placeholders are invocation fallback only. They do not replace project-backed API Workflow creation, solution registration, build validation, or upload verification.

## Validation Gates

- Payload JSON is valid.
- Workflow JSON is valid.
- Output schema matches the payload shape.
- Every mapped downstream JSON path exists in the payload.
- `uip api-workflow run <Workflow.json> --no-auth` returns the expected mock payload, unless the API workflow tool is unavailable and the skip is recorded.
- `project.uiproj` exists with `ProjectType` set to `Api` and `MainFile` set to `Workflow.json`.
- `entry-points.json` exists with `type` set to `Api` and an output schema matching the mock payload.
- `bindings_v2.json` and `.local/ProjectSettings.json` exist.
- `uip api-workflow build <projectPath> --output json` returns success.
- The solution `.uipx` contains one `Type: "Api"` project entry per planned `API-*`.
- No secrets, real personal data, real account numbers, or live endpoints appear in payloads.
- Handoff docs clearly label every API workflow as a mock passthrough resource.

## Validation State Model

| State | Meaning | Sufficient for Studio Web demo? |
|---|---|---|
| JSON valid | `Workflow.json` parses | No |
| Local run passed | `uip api-workflow run ... --no-auth` works | No |
| Project build passed | `uip api-workflow build <project>` works | No |
| Solution registered | `.uipx` contains `Type: "Api"` entry | Required before upload |
| Upload confirmed | `uip solution upload` response contains `projectType: "Api"` | Required before final handoff |

## Handoff To Flow

For each `API-*`, provide:

- API workflow display name.
- API project directory.
- `project.uiproj` path.
- `entry-points.json` path and entry point `uniqueId`.
- Solution manifest project ID from the `.uipx`.
- Upload response project name and `projectType`, when available.
- Node label.
- Expected registry node type: `uipath.core.api-workflow.<resourceKey>`.
- Binding subtype: `Api`.
- Binding orchestrator type: `api`.
- Output variable shape: `$vars.<apiNodeId>.output`.
- JSON paths consumed by agents, conditions, connectors, and End outputs.
- Local run and project build validation results.
- Flow invocation status: `bound API node`, `script placeholder`, or `not used`.
- Registry discovery status or native binding blocker.

## Templates

- `templates/api-workflow-contract.template.md`
- `templates/mock-response-payload.template.json`
- `templates/passthrough-api-workflow.template.json`
- `templates/project.uiproj.template.json`
- `templates/entry-points.template.json`
- `templates/bindings_v2.template.json`
- `templates/project-settings.template.json`
