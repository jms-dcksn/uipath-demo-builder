# Component Specifications - KYC Onboarding

## 1) Component Index

| Component ID | Component Type | Task IDs | Owner | Build Status |
|---|---|---|---|---|
| CMP-IDP-01 | IDP | T-002 | Document AI Team | Planned |
| CMP-API-01 | API | T-004 | Integration Team | Planned |
| CMP-API-02 | API | T-006 | Integration Team | Planned |
| CMP-AG-01 | AI Agent | T-003 | AI Team | Planned |
| CMP-AG-02 | AI Agent | T-005 | AI Team | Planned |
| CMP-AG-03 | AI Agent | T-007 | AI Team | Planned |
| CMP-RPA-01 | RPA | T-009 | Automation Team | Planned |
| CMP-API-03 | API | T-010, T-011 | Integration Team | Planned |

## 2) Component Spec

### Component: `CMP-IDP-01`

- Type: `IDP`
- Business purpose: Extract key KYC fields from uploaded documents.
- Trigger: Case enters T-002.
- Inputs: Formation docs, identity docs.
- Outputs: Structured field map + per-field confidence.
- Upstream dependencies: Case/document ingestion complete.
- Downstream dependencies: Completeness agent T-003.
- Exception cases: Unreadable docs, unsupported format.
- Retry/idempotency strategy: Retry extraction once; then route to manual extraction task.
- Logging/audit requirements: Store extracted fields, confidence, extraction model version.
- Security/credential requirements: Access to document store with least privilege.
- SLA target: 3 minutes per document bundle.

### Component: `CMP-API-02`

- Type: `API`
- Business purpose: Run sanctions/watchlist screening for customer and beneficial owners.
- Trigger: T-006.
- Inputs: Legal entity and party identifiers.
- Outputs: Hit list with match scores and metadata.
- Upstream dependencies: Validated customer profile.
- Downstream dependencies: T-007 reviewer briefing and T-008 adjudication.
- Exception cases: Provider timeout, partial response.
- Retry/idempotency strategy: Retry up to 3 times, then create exception task.
- Logging/audit requirements: Persist query timestamp and list version.
- Security/credential requirements: API key/secret in secure vault.
- SLA target: < 60 seconds per screening call.

### Component: `CMP-RPA-01`

- Type: `RPA`
- Business purpose: Update legacy core banking customer profile after approval.
- Trigger: T-009 on approved decision.
- Inputs: Approved case payload.
- Outputs: Core update confirmation ID.
- Upstream dependencies: T-008 approved decision.
- Downstream dependencies: T-011 closure.
- Exception cases: UI selector drift, session timeout.
- Retry/idempotency strategy: One automatic retry; then route to ops exception task.
- Logging/audit requirements: Record target system confirmation reference.
- Security/credential requirements: Robot account with scoped access.
- SLA target: < 5 minutes per update.

## 3) API Contract Stubs

| Component ID | Endpoint/Operation | Method | Request Shape | Response Shape | Error Codes |
|---|---|---|---|---|---|
| CMP-API-01 | `/registry/verify-entity` | `POST` | `{ legalEntityName, registrationNumber }` | `{ verified, confidence, registryRef }` | `400`, `429`, `503` |
| CMP-API-02 | `/screening/sanctions-check` | `POST` | `{ parties: [...] }` | `{ status, hits: [...], listVersion }` | `400`, `401`, `504` |
| CMP-API-03 | `/notifications/case-decision` | `POST` | `{ caseId, decision, recipients }` | `{ sent, messageIds }` | `400`, `429`, `500` |

## 4) RPA Interaction Notes

| Component ID | App/System | UI Steps Summary | Selectors/Anchors Notes | Failure Recovery |
|---|---|---|---|---|
| CMP-RPA-01 | Legacy Core Banking UI | Login -> search legal entity -> create/update profile -> submit | Anchor on registration number field + confirmation banner | Retry once; if failed create ops exception task |

## 5) IDP Extraction Notes

| Component ID | Document Type | Required Fields | Confidence Threshold | Escalation Rule |
|---|---|---|---|---|
| CMP-IDP-01 | Certificate of formation | legalEntityName, registrationNumber, incorporationCountry | 0.92 | Any required field below threshold -> manual review |
| CMP-IDP-01 | Beneficial ownership declaration | ownerName, ownershipPct, ownerDOB | 0.90 | Missing owner identity fields -> PendingInfo |
