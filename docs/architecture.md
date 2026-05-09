# Architecture

UPP follows a simple resource-oriented architecture with three layers:

## 1. Core protocol layer

Defines common object envelopes, identifiers, timestamps, parties, versioning, status values, and extension rules.

## 2. Domain modules

Modules standardize specific procurement interactions:

- UPP Core
- UPP Capabilities
- UPP RFQ
- UPP Orders
- UPP Catalog
- UPP Approval
- UPP Invoice
- UPP Identity
- UPP Extensions

## 3. Transport and integration layer

UPP is transport-friendly rather than transport-prescriptive. Implementations can expose UPP through:

- HTTPS APIs
- message queues or event buses
- ERP adapters
- EDI gateway mappings
- procurement platform connectors

This allows a UPP workflow to coexist with ERP approvals, EDI invoice delivery, and vendor-specific back-office systems.

## Architectural model

UPP borrows a few ideas from MCP without turning procurement systems into tool execution frameworks:

- machine-readable capabilities are discoverable
- schemas define valid inputs and outputs
- resources are explicit and versioned
- extensions are namespaced
- unsafe arbitrary execution is out of scope

The protocol should let systems exchange procurement intent and state, not remote shell commands.
