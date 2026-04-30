# Connector Bindings

## 1) Connector Inventory

| Connector ID | Service | Connector Key | Activity | Connection Required? | Status |
|---|---|---|---|---|---|
| CONN-001 |  |  |  | Yes | Pending |

## 2) Connection Discovery

| Connector ID | Command | Connection ID | Folder Key | State | Notes |
|---|---|---|---|---|---|
| CONN-001 | `uip is connections list <connector-key> --output json` |  |  |  |  |

## 3) Required Field Resolution

| Connector ID | Field | Required | Source | Value / Expression | Notes |
|---|---|---|---|---|---|
| CONN-001 |  | Yes | user / fixture / upstream node |  |  |

## 4) Reference Field Resolution

| Connector ID | Field | Lookup Command | Resolved ID | Display Value |
|---|---|---|---|---|
| CONN-001 |  |  |  |  |

## 5) Configuration Detail

| Connector ID | Method | Endpoint | Body Parameters | Query Parameters | Path Parameters |
|---|---|---|---|---|---|
| CONN-001 |  |  |  |  |  |

## 6) Blockers

| Connector ID | Blocker | Required User Action |
|---|---|---|
| CONN-001 | No enabled connection | Create or select an Integration Service connection |

## 7) Scope Notes

- Connector-backed managed HTTP requires explicit user approval as a connector workaround.
- Manual HTTP is out of the default local-execution scope.
