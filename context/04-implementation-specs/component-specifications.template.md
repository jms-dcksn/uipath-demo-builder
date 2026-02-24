# Component Specifications Template

Use this for `RPA`, `API`, and `IDP` components that may be implemented in proprietary tooling.

## 1) Component Index

| Component ID | Component Type | Task IDs | Owner | Build Status |
|---|---|---|---|---|
| CMP-RPA-01 | RPA | T-010, T-011 |  | Planned |

## 2) Component Spec

Copy this section once per component.

### Component: `<Component ID>`

- Type: `RPA | API | IDP`
- Business purpose:
- Trigger:
- Inputs:
- Outputs:
- Upstream dependencies:
- Downstream dependencies:
- Exception cases:
- Retry/idempotency strategy:
- Logging/audit requirements:
- Security/credential requirements:
- SLA target:

## 3) API Contract Stubs

| Component ID | Endpoint/Operation | Method | Request Shape | Response Shape | Error Codes |
|---|---|---|---|---|---|
| CMP-API-02 | `/applications/{id}` | `PATCH` |  |  |  |

## 4) RPA Interaction Notes

| Component ID | App/System | UI Steps Summary | Selectors/Anchors Notes | Failure Recovery |
|---|---|---|---|---|
| CMP-RPA-01 | Legacy CRM | Login -> search -> update -> confirm | Stable anchor on case ID | Retry once then create exception task |

## 5) IDP Extraction Notes

| Component ID | Document Type | Required Fields | Confidence Threshold | Escalation Rule |
|---|---|---|---|---|
| CMP-IDP-03 | Application Form | ApplicantName, Amount, Date | 0.92 | Route low confidence to manual validation |

