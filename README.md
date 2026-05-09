# B2B Protocol

B2B Protocol is developing UPP, the Universal Procurement Protocol.

UPP is a draft interoperability layer for AI-native B2B procurement. It focuses on machine-readable procurement workflows that can be implemented through JSON schemas, HTTP APIs, and adapters that connect to existing ERP, procurement, and integration systems.

## What is in this repo

- `docs/`: developer documentation and the initial protocol draft
- `docs/research-notes.md`: source-backed rationale for the first protocol cut
- `schemas/`: JSON Schemas for the first core objects
- `openapi/`: draft HTTP API surface
- `examples/`: example capability and workflow payloads
- `website/landing-page/`: initial landing page for `b2bprotocol.com`

## Initial scope

The current draft focuses on a minimal procurement workflow:

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
- Clear versioning
- Secure by default
- Human-readable and machine-readable
- Compatible with existing ERP and procurement systems
- Agent-friendly and enterprise-safe
