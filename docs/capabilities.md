# Capabilities

This document defines the canonical capability identifiers for the first UPP procurement modules.

Capability identifiers are stable negotiation names. They are not endpoint names and they are not transport-specific method names.

## Naming rules

- Use reverse-domain naming under `com.b2bprotocol`.
- Follow the UCP baseline format: `{reverse-domain}.{service}.{capability}`.
- Use capability names for semantic units, not transport verbs.
- Version each capability independently.
- Advertise capabilities in `CapabilityProfile.capabilities`.

## Core capability set

The first service name is:

- `com.b2bprotocol.procurement`

Capabilities for that service are:

### `com.b2bprotocol.procurement.intent`

Declares support for representing normalized procurement intent.

Typical meaning:

- accepts or exposes `Intent`
- supports structured buyer need representation
- supports downstream discovery and transaction flows

Module:

- `UPP Discovery`

### `com.b2bprotocol.procurement.discovery`

Declares support for discovery-first supplier matching from structured buyer intent.

Typical meaning:

- accepts `DiscoveryRequest`
- returns `DiscoveryResponse`
- exposes supplier fit before formal RFQ

Module:

- `UPP Discovery`

### `com.b2bprotocol.procurement.offering`

Declares support for publishing searchable supplier offerings.

Typical meaning:

- exposes `Offering` records
- supports deliverable/category matching
- may provide regional or commercial constraints

Module:

- `UPP Discovery`

### `com.b2bprotocol.procurement.rfq`

Declares support for structured RFQ submission and quote retrieval.

Typical meaning:

- accepts `RFQ`
- returns or exposes `Quote`
- supports direct buyer-to-supplier quote exchange

Module:

- `UPP RFQ`

### `com.b2bprotocol.procurement.order`

Declares support for structured purchase order submission.

Typical meaning:

- accepts `PurchaseOrder`
- exposes order state
- may reference quote-derived or discovery-derived orders

Module:

- `UPP Orders`

### `com.b2bprotocol.procurement.invoice`

Declares support for structured invoice submission and retrieval.

Typical meaning:

- accepts `Invoice`
- exposes invoice match state
- supports downstream reconciliation workflows

Module:

- `UPP Invoice`

### `com.b2bprotocol.procurement.approval`

Declares support for reading approval workflow status.

Typical meaning:

- exposes `ApprovalStatus`
- supports human-in-the-loop enterprise workflows

Module:

- `UPP Approval`

## Versioning guidance

For the current draft, implementations should advertise:

- `version: "0.2.0-draft"`

Capability versions may diverge later from the top-level `specVersion`, but the initial draft should keep them aligned unless there is a clear compatibility reason not to.

## Negotiation guidance

A client should:

1. Fetch `/.well-known/upp`
2. Read `services`
2. Read `capabilities`
3. Compute the intersection of supported capability names and versions
4. Let the server select the active capability set from that intersection
5. Use the matching binding and endpoints for the chosen capability set
