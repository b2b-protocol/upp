# Core Concepts

## Parties

UPP models two primary party types:

- `Buyer`
- `Vendor`

Each party has a stable identifier, display name, contact metadata, and optional external system references.

## Capability Document

A `CapabilityDocument` is the entry point for discovery. It describes:

- which UPP modules are supported
- which API endpoints are available
- which identity and auth methods are required
- which extensions are exposed
- which workflow constraints apply

## Workflow Objects

The initial protocol defines these workflow objects:

- `RFQ`
- `Quote`
- `PurchaseOrder`
- `Invoice`
- `ApprovalStatus`
- `PaymentTerms`
- `Extension`

## Extension model

Extensions use namespaced keys, for example:

- `com.b2bprotocol.procurement.rfq`
- `com.b2bprotocol.procurement.purchase_order`
- `com.vendorname.custom_pricing`

Unknown extensions must be safely ignored unless explicitly marked as required.

## Versioning

Each object carries version information:

- `specVersion`: the UPP version
- `schemaVersion`: the schema release when applicable

Breaking changes should increment the major version.
