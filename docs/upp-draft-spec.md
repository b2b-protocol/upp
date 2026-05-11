# UPP Draft Specification

Version: `0.2.0-draft`

## 1. Scope

UPP defines a minimal set of AI-native procurement workflows between buyers, vendors, marketplaces, procurement tools, and enterprise systems.

This draft covers:

- source document registration
- procurement intent extraction and normalization
- discovery query execution and result exchange
- capability discovery
- RFQ submission
- quote exchange
- purchase order submission
- approval status reporting
- invoice exchange and matching
- buyer and vendor identity
- namespaced extensions

This draft does not attempt to replace EDI, standardize every procurement document, or define a universal ERP data model.

This draft follows the UCP baseline where practical: profile discovery through a well-known endpoint, services and capabilities declared separately, capability negotiation before action, server-selects intersection, transport-agnostic semantics, and canonical schemas referenced by transport bindings rather than duplicated inside them.

## 2. Goals

- reduce custom procurement integrations
- define machine-readable procurement workflows
- support AI agents without requiring unsafe autonomy
- preserve compatibility with enterprise systems
- keep the core small and extensible
- standardize discovery before transaction

## 3. Object envelope

All top-level UPP objects use a shared envelope:

```json
{
  "specVersion": "0.2.0-draft",
  "id": "string",
  "type": "string",
  "createdAt": "2026-05-11T10:00:00Z",
  "updatedAt": "2026-05-11T10:00:00Z",
  "extensions": {}
}
```

## 4. Core object definitions

### Vendor

A selling party that exposes a capability profile and accepts one or more UPP workflows.

### Buyer

A purchasing party that expresses buying intent, requests quotes, submits purchase orders, and receives invoices.

### SourceArtifact

A source procurement artifact such as a PDF specification, statement of work, bill of quantities, spreadsheet, email package, or requirements pack. A `SourceArtifact` records provenance and file metadata for later extraction and auditability.

### Intent

A normalized representation of buyer need. An `Intent` captures deliverables, quantities, timelines, delivery targets, constraints, and commercial signals independently of any one supplier.

### Constraint

A structured hard or soft requirement attached to an `Intent` or `DiscoveryRequest`, such as geography, certification, lead time, budget ceiling, or payment method.

### Offering

A search-friendly supplier description of deliverables, categories, commercial patterns, and fulfillment constraints. This object complements a `CapabilityProfile` by describing what a supplier can provide rather than how to call its API.

### CapabilityProfile

Machine-readable description of protocol support, auth requirements, endpoints, constraints, and extensions.

At minimum, a profile should declare:

- supported service identifiers
- supported capability identifiers
- capability versions
- supported transport bindings
- auth requirements
- discovery and transaction endpoints
- public signing keys when signed transports are supported

### DiscoveryRequest

A network-searchable request derived from an `Intent`. It asks which connected suppliers or marketplaces can satisfy a buying need before a formal RFQ is sent.

### DiscoveryResponse

A result set returned in response to a `DiscoveryRequest`. It includes matched suppliers, relevant offerings, constraints, fit signals, and next actions.

### MatchReason

A structured reason describing why a supplier or offering matched a discovery request. This is intended to support agent transparency and buyer review.

### AvailableAction

A structured next-step action attached to a discovery or workflow result. An `AvailableAction` tells an agent what it may do next, which resource that action targets, and whether human approval is required before execution.

### BindingState

A structured declaration of commercial binding strength for a result or transaction. A `BindingState` tells an agent whether the current state is exploratory, indicative, quoted, approved, or committed, and whether it should be treated as commercially binding.

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

### UPP Discovery

Source artifact registration, intent representation, discovery request exchange, and explainable discovery responses.

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

## 5.1 Canonical first-draft capabilities

The initial canonical service identifier is:

- `com.b2bprotocol.procurement`

The initial canonical capability identifiers are:

- `com.b2bprotocol.procurement.intent`
- `com.b2bprotocol.procurement.discovery`
- `com.b2bprotocol.procurement.offering`
- `com.b2bprotocol.procurement.rfq`
- `com.b2bprotocol.procurement.order`
- `com.b2bprotocol.procurement.invoice`
- `com.b2bprotocol.procurement.approval`

These capability names are intended for profile negotiation and should be advertised with explicit versions in `CapabilityProfile.capabilities`.

## 6. Discovery-first workflow

The expected discovery-first workflow is:

1. Register a `SourceArtifact`.
2. Create or derive an `Intent`.
3. Execute a `DiscoveryRequest`.
4. Review `DiscoveryResponse` matches and reasons.
5. Convert selected matches into RFQs, purchase orders, or internal workflows.

Implementations may still create RFQs directly when a buyer already knows the supplier.

## 7. Interoperability rules

- Unknown optional fields must not break processing.
- Unknown required extensions must fail fast with a clear error.
- Mutating operations should support idempotency.
- Capability discovery should be exposed at `/.well-known/upp`.
- Profiles that support multiple protocol versions SHOULD advertise version-specific profile URLs.
- Discovery results should reference the originating intent or query.
- Implementations may map UPP objects into internal ERP, procurement, or EDI systems without changing the external UPP contract.
- Service identifiers SHOULD follow the same reverse-domain naming rules as capability identifiers.
- Capability identifiers should use reverse-domain naming when governance matters.
- Capability profiles should declare capability names and versions explicitly.
- Capability profiles should declare services explicitly.
- Capability profiles should declare available transport bindings explicitly.
- Transport bindings should reference canonical schemas and should not redefine payload shapes inline.
- Extensions should compose onto base schemas rather than replace them.

## 8. Example extension keys

- `com.b2bprotocol.discovery.search`
- `com.b2bprotocol.rfq.exchange`
- `com.b2bprotocol.orders.submit`
- `com.b2bprotocol.invoice.submit`
- `com.vendorname.custom_pricing`

## 9. Security requirements

- Validate every top-level object against schema.
- Do not treat natural-language instructions as executable authority.
- Bind approval-sensitive actions to explicit system policy.
- Authenticate counterparties using enterprise-safe mechanisms.
- Preserve provenance between source documents, extracted intent, and downstream transactions.
- For HTTP-based signed transports, use RFC 9421 HTTP Message Signatures with `Content-Digest` over raw body bytes.
- Publish public signing keys in the capability profile when signed transports are supported.
- For streamable HTTP MCP, sign the same HTTP envelope fields as REST and treat the JSON-RPC body as the signed content body.
- Use `UPP-Agent` to advertise the caller profile URL on signed HTTP requests.

## 10. Implementation note

UPP is intended to map cleanly into:

- ERP adapters
- procurement suites
- API gateways
- EDI translation layers
- marketplace integrations
- discovery and matching services

## 11. Transport bindings

UPP core semantics are transport-agnostic.

Concrete transport bindings may be defined for:

- REST
- MCP
- A2A

Those bindings should all point back to the same canonical schema set and capability profile model.

The MCP binding should expose action-oriented tools and readable resources while keeping UPP objects as the authority layer for workflow state, approval boundaries, and commercial binding strength.

## 12. Schema composition and resolution

UPP should follow the UCP baseline for schema mechanics where practical.

The preferred model is:

- self-describing payload metadata under top-level `upp`
- schema composition through capability negotiation
- operation-specific resolution for request and response views
- runtime validation against resolved schemas

Recommended annotation names for future schema tooling are:

- `upp_request`
- `upp_response`

Recommended operation names are:

- `create`
- `read`
- `update`
- `complete`

Recommended visibility values are:

- `omit`
- `optional`
- `required`

These rules are intended to support a future `upp-schema` toolchain analogous to `ucp-schema`, while keeping the protocol semantics procurement-specific.
