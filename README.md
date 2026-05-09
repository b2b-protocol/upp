# B2B Protocol

B2B Protocol is building UPP, the Universal Procurement Protocol: an interoperability layer for AI-native B2B procurement.

UPP is designed to help AI agents, buyers, vendors, marketplaces, and enterprise systems exchange procurement workflows through a small core, machine-readable schemas, and extensible APIs. It is not a replacement for EDI on day one. It is a pragmatic protocol layer that can coexist with ERP, procurement, and integration infrastructure already in production.

## What is in this repo

- `docs/`: developer documentation and the initial protocol draft
- `docs/research-notes.md`: source-backed rationale for the first protocol cut
- `schemas/`: JSON Schemas for the first core objects
- `openapi/`: draft HTTP API surface
- `examples/`: example capability and workflow payloads
- `website/landing-page/`: initial landing page for `b2bprotocol.com`

## Initial scope

The first UPP draft focuses on a minimal procurement workflow:

- Vendor capability discovery
- RFQ request and response
- Purchase order creation
- Approval workflow status
- Invoice matching
- Payment terms
- Buyer and vendor identity
- Vendor-specific extensions

## Design principles

- Small core, extensible edges
- JSON Schema first
- OpenAPI-compatible
- Versioned and implementation-first
- Secure by default
- Human-readable and machine-readable
- Compatible with existing ERP and procurement systems
- Agent-friendly and enterprise-safe
