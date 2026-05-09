# UPP Draft Specification

Version: `0.1.0-draft`

## 1. Scope

UPP defines a minimal set of AI-native procurement workflows between buyers, vendors, marketplaces, procurement tools, and enterprise systems.

This draft covers:

- capability discovery
- RFQ submission
- quote exchange
- purchase order submission
- approval status reporting
- invoice exchange and matching
- buyer and vendor identity
- namespaced extensions

This draft does not attempt to replace EDI, standardize every procurement document, or define a universal ERP data model.

## 2. Goals

- reduce custom procurement integrations
- define machine-readable procurement workflows
- support AI agents without requiring unsafe autonomy
- preserve compatibility with enterprise systems
- keep the core small and extensible

## 3. Object envelope

All top-level UPP objects use a shared envelope:

```json
{
  "specVersion": "0.1.0-draft",
  "id": "string",
  "type": "string",
  "createdAt": "2026-05-09T10:00:00Z",
  "updatedAt": "2026-05-09T10:00:00Z",
  "extensions": {}
}
```

## 4. Core object definitions

### Vendor

A selling party that exposes a capability document and accepts one or more UPP workflows.

### Buyer

A purchasing party that requests quotes, submits purchase orders, and receives invoices.

### CapabilityDocument

Machine-readable description of protocol support, auth requirements, endpoints, constraints, and extensions.

### RFQ

Structured request for goods or services.

### Quote

Vendor response to an RFQ with pricing, terms, validity, and assumptions.

### PurchaseOrder

Structured commitment to purchase, optionally derived from a quote.

### Invoice

Structured bill referencing one or more prior procurement objects.

### ApprovalStatus

Workflow status object for human or system approval state.

### PaymentTerms

Shared representation for due dates, settlement windows, discounts, and payment methods.

### Extension

Namespaced additional data attached to a base object.

## 5. Modules

### UPP Core

Shared object envelope, IDs, timestamps, versioning, and extensions.

### UPP Capabilities

Capability discovery and support declaration.

### UPP RFQ

Request-for-quote and quote exchange.

### UPP Orders

Purchase order creation and status.

### UPP Catalog

Optional product or service listing references.

### UPP Approval

Approval status and approver context.

### UPP Invoice

Invoice representation and match state.

### UPP Identity

Buyer and vendor identity references.

### UPP Extensions

Namespaced custom fields.

## 6. Interoperability rules

- Unknown optional fields must not break processing.
- Unknown required extensions must fail fast with a clear error.
- Mutating operations should support idempotency.
- Capability discovery should be exposed at a stable URL.
- Implementations may map UPP objects into internal ERP, procurement, or EDI systems without changing the external UPP contract.

## 7. Example extension keys

- `com.b2bprotocol.procurement.rfq`
- `com.b2bprotocol.procurement.purchase_order`
- `com.vendorname.custom_pricing`

## 8. Security requirements

- Validate every top-level object against schema.
- Do not treat natural-language instructions as executable authority.
- Bind approval-sensitive actions to explicit system policy.
- Authenticate counterparties using enterprise-safe mechanisms.

## 9. Implementation note

UPP is intended to map cleanly into:

- ERP adapters
- procurement suites
- API gateways
- EDI translation layers
- marketplace integrations
