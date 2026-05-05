# Connector Bindings

## 1) Connector Inventory

| Connector ID | Service | Connector Key | Activity | Connection Required? | Status |
|---|---|---|---|---|---|
| CONN-001 |  |  |  | Yes | Pending |

## 2) Connection Discovery

| Connector ID | Lookup Command | Connection ID | Folder Key | State | Ping Result | Notes |
|---|---|---|---|---|---|---|
| CONN-001 | `uip is connections list <connector-key> --folder-key <folder-key> --output json` |  |  |  | Pending |  |

## 3) Enriched Metadata

| Connector ID | Node Type | Registry Command | Metadata Source | Method | Endpoint |
|---|---|---|---|---|---|
| CONN-001 | `uipath.connector.<connector-key>.<activity>` | `uip maestro flow registry get <nodeType> --connection-id <connection-id> --output json` | registry get / `uip is resources describe` |  |  |

## 4) Required Field Resolution

| Connector ID | Field | Required | Source | Value / Expression | Notes |
|---|---|---|---|---|---|
| CONN-001 |  | Yes | user / fixture / upstream node |  |  |

## 5) Reference Field Resolution

| Connector ID | Field | Lookup Command | Resolved ID | Display Value | Status |
|---|---|---|---|---|---|
| CONN-001 |  |  |  |  | Pending |

## 6) Parameter Mapping

| Connector ID | Body Parameters | Query Parameters | Path Parameters | Output Reference | Downstream Consumers |
|---|---|---|---|---|---|
| CONN-001 |  |  |  | `$vars.<nodeId>.output` |  |

## 7) Blockers

| Connector ID | Blocker | Required User Action |
|---|---|---|
| CONN-001 | No enabled connection | Create or select an Integration Service connection |

## 8) Scope Notes

- Connector implementation mechanics live in `uipath-maestro-flow/references/author/references/plugins/connector/impl.md`.
- Connector-backed managed HTTP requires explicit user approval as a connector workaround.
- Manual HTTP is out of the default local-execution scope.
- Mock API workflows are deterministic passthrough resources, not connector substitutes for live writes.
- Use `=js:` for every `$vars`, `$metadata`, or `$self` reference inside connector body, query, or path parameter values.
- If a mock API Workflow project cannot be bound as a native Flow node yet, document the script-backed placeholder in `flow/registry-discovery.md`.
- Script placeholders are invocation stand-ins only; they do not replace API Workflow project build, solution registration, or Studio Web upload verification.
