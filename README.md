# B2B Protocol

B2B Protocol is developing UPP, the Universal Procurement Protocol.

UPP is a draft interoperability layer for AI-native B2B procurement. It focuses on machine-readable procurement workflows that can be implemented through JSON schemas, HTTP APIs, and adapters that connect to existing ERP, procurement, and integration systems.

## Overview

The current draft focuses on a small set of procurement workflows:

- vendor capability discovery
- RFQ request and response
- purchase order creation
- approval workflow status
- invoice matching
- payment terms
- buyer and vendor identity
- vendor-specific extensions

## Repository structure

- `docs/`: specifications, architecture notes, and protocol documentation
- `schemas/`: JSON Schemas for initial core objects
- `openapi/`: draft HTTP API surface
- `examples/`: example payloads and workflow data
- `website/landing-page/`: initial landing page assets

## Key documents

- [Documentation index](docs/index.md)
- [UPP draft specification](docs/upp-draft-spec.md)
- [Research notes](docs/research-notes.md)

## Design principles

- small core, extensible edges
- JSON Schema first
- OpenAPI-compatible APIs
- clear versioning
- secure by default
- human-readable and machine-readable
- compatible with existing ERP and procurement systems
- agent-friendly and enterprise-safe
