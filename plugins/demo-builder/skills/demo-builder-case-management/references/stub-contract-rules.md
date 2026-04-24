# Non-Agent Stub Contract Rules

Every task in a Case Management design that is **not** `AI Agent` and **not** `Human Task` — plus the single `Trigger` and any `Intermediate Event`s — must have a **stub contract** authored at design time. Stubs are what let the user stand up a pass-through RPA / API / IDP workflow in minutes for a demo, then drop in the real implementation later without reshaping the case flow, agents, or frontend.

## Why at design time (not Phase 6)

The frontend reads these outputs. Agents that sit downstream read these outputs. If the I/O shape is not pinned during case design, every downstream builder invents its own shape and mock/real swap becomes a rewrite. Pinning the shape now is the whole point.

## One stub, one row

Each stub is a single row in the "Non-Agent Stub Contracts" table of `case-management-design.template.md`. A row has:

| Field | Meaning |
|---|---|
| `CMP-<TYPE>-##` | Component ID. `<TYPE>` ∈ `RPA \| API \| IDP \| TRG \| EVT`. |
| Backs task | The `T-###` this stub implements (or `—` for `TRG` / `EVT`). |
| Stage | The case stage the stub fires in. |
| Type | `RPA` / `API` / `IDP` / `Trigger` / `Intermediate Event`. May include a connector, e.g. `API (Connector: Salesforce)`. |
| Inputs | Field name · type · source (case entity field / prior task output / trigger payload) |
| Outputs | Field name · type · which case-entity field the output writes to (if any) |
| Demo values — happy | Hardcoded input values + hardcoded output values for the happy path |
| Demo values — exception | Hardcoded input values + hardcoded output values for the one exception path |
| Mock hint | One sentence. How a demo implementer should fake this — e.g., *"pass-through: ignore inputs; return hardcoded `{policyId:'POL-12345', riskTier:'LOW'}` on happy, `{riskTier:'HIGH'}` on exception"*. |
| Real-build note | One sentence describing what the user will eventually wire — e.g., *"calls Salesforce REST `/services/data/v59.0/sobjects/Account/{id}` with OAuth; swap the mock tool out when credentials land"*. |

## Rules

1. **Schema is mandatory, values can be `TBD`.** Field names and types must be concrete. Hardcoded demo values may be marked `TBD <owner/date>` but only if the demo script doesn't need the value visible yet.
2. **Happy + one exception, no more.** Two demo paths. If the use case needs three, merge or cut.
3. **Identical interfaces.** A stub's input/output shape must match what the real component will eventually produce. If unsure of real shape, pick the smallest credible shape — don't invent fields.
4. **Writes flow into the case entity.** Every stub output field that is meant to persist must map to a field in the Data Fabric case entity schema. If it doesn't map, either add it to the entity or don't persist it.
5. **Trigger is always present.** Exactly one `CMP-TRG-01` row. Specify trigger type (email / form / webhook / schedule / queue) and the inbound payload schema + a hardcoded demo payload per path.
6. **Intermediate Events are optional.** Use `CMP-EVT-##` only when the flow actually waits mid-stage (timer expiry, inbound message, signal). Skip otherwise.
7. **Agents read stubs, don't own them.** If an `AG-*` agent consumes output from a stub, reference the stub ID in the agent's design brief input contract. Do not duplicate the shape.
8. **Never fabricate connection-ids or task-type-ids.** Those come from the production `uipath-case-management` skill's registry at generation time. Stubs stay symbolic until that skill resolves them.

## Example stub row

```
| CMP-API-01 | T-003 | Processing | API (Connector: Policy Admin)
  | inputs:  policyNumber:string (from case.policyNumber)
  | outputs: policyStatus:string → case.policyStatus,
             premiumAmount:number → case.premiumAmount
  | happy:  in {policyNumber:'POL-12345'}  out {policyStatus:'ACTIVE',  premiumAmount:1450.00}
  | excep:  in {policyNumber:'POL-99999'}  out {policyStatus:'LAPSED', premiumAmount:0}
  | mock:   pass-through lookup table on policyNumber; two rows only
  | real:   calls internal Policy Admin REST GET /policies/{id}; OAuth client creds
```

## How downstream skills use stubs

- `demo-builder-frontend` renders stub outputs like any other case entity field — UI never knows a stub is mocked.
- `agent-builder` sub-agents reference upstream stub IDs in their input contracts; tool wrappers match the stub's output shape.
- `uipath-case-management` receives these as skeleton tasks in `sdd.md` (type + display name + declared I/O where resolvable); it inserts `<UNRESOLVED: ...>` markers for task-type-ids and connection-ids that must be picked in the Studio Web registry.
