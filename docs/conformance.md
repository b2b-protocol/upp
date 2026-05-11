# Conformance

This document defines the first conformance direction for UPP.

At this stage, conformance means proving that an implementation:

- publishes a valid profile
- exchanges canonical objects
- preserves workflow lineage
- respects capability negotiation and approval boundaries

## Conformance levels

The first draft should use lightweight conformance levels.

### Level 1: Profile and schema conformance

An implementation:

- publishes a valid `CapabilityProfile`
- uses canonical capability names and versions
- returns payloads that conform to the published schemas

### Level 2: Discovery workflow conformance

An implementation:

- supports `Intent`
- supports `DiscoveryRequest`
- returns `DiscoveryResponse`
- includes `MatchReason`
- includes `AvailableAction`
- includes `BindingState` where required

### Level 3: Transaction workflow conformance

An implementation:

- supports `RFQ`
- supports `Quote`
- supports `PurchaseOrder`
- supports `Invoice`
- supports `ApprovalStatus`
- preserves lineage across those objects

### Level 4: Signed transport conformance

An implementation:

- publishes signing keys in profile when signed transports are used
- verifies signed HTTP requests correctly
- preserves `UPP-Agent`, `Content-Digest`, and signature behavior across REST or MCP over streamable HTTP

## Required conformance checks

The first conformance suite should check:

- profile retrieval from `/.well-known/upp`
- schema validity of example request and response payloads
- presence of required references such as `intentId`, `rfqId`, `quoteId`, and `purchaseOrderId` where applicable
- correct distinction between capability discovery and supplier-fit discovery
- explicit approval and binding semantics

## Example smoke checks

### Capability profile

- can fetch `/.well-known/upp`
- profile declares service `com.b2bprotocol.procurement`
- profile declares canonical capabilities and versions

### Discovery

- `DiscoveryResponse` contains at least one supplier match
- each match can include `MatchReason`
- each match can include `AvailableAction`
- discovery results remain non-binding or indicative unless stated otherwise

### RFQ and quote

- `RFQ` references upstream context where available
- `Quote` references `rfqId`
- `Quote` exposes `BindingState`

### Purchase order and approval

- `PurchaseOrder` references upstream commercial lineage where available
- `ApprovalStatus` is readable as a separate object
- approval state is not silently inferred from order status alone

### Invoice

- `Invoice` references `purchaseOrderId` where applicable
- `matchStatus` is explicit

## Current recommendation

For the current draft:

- start with example-driven conformance checks
- add schema validation first
- add signed transport conformance next
- only then move toward a fuller automated suite
