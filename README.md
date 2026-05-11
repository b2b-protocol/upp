# B2B Protocol

B2B Protocol is developing UPP, the Universal Procurement Protocol.

UPP is a procurement-domain interoperability protocol for AI-native B2B procurement.

It follows the same broad architectural posture as UCP:

- machine-readable profiles
- capability negotiation before action
- canonical schemas for semantic meaning
- transport-agnostic core workflows

The difference is domain specialization.

UPP is not a generic commerce protocol.

It is focused on procurement interactions between buyers, vendors, procurement platforms, marketplaces, and LLM-driven agents.

## Why it exists

Traditional B2B protocols standardized document transport and partner connectivity.

They remain important, but they do not provide a shared machine-readable model for:

- procurement intent
- supplier discovery
- approval-sensitive actions
- commercially meaningful workflow state

across AI agents, buyer systems, vendor systems, and marketplaces.

UPP is intended to provide that shared language.

## Current direction

The current draft is discovery-first.

UPP starts earlier than a traditional procure-to-pay workflow:

1. Register a source artifact such as a PDF specification or scope of work.
2. Extract structured procurement intent from that source.
3. Search connected suppliers and marketplaces for matching capabilities and deliverables.
4. Move matched opportunities into RFQ, ordering, approval, and invoice workflows.

This means UPP is not just about exchanging downstream procurement documents.

It is also about making upstream supplier discovery and procurement reasoning machine-usable.

## Current scope

The initial draft provides a shared model for:

- publishing and consuming capability profiles
- negotiating supported procurement capabilities and versions
- registering source procurement artifacts
- representing structured procurement intent
- expressing discovery requests and matched results
- exposing searchable supplier offerings
- submitting RFQs and retrieving quotes
- creating purchase orders
- reporting approvals and workflow state
- reconciling invoices and payment terms
- attaching namespaced extensions without breaking interoperability

## Agent-safe by design

UPP is intended to be usable by LLMs and software agents.

That means the protocol should not rely on freeform text alone to decide what happens next.

Instead, it should make key workflow boundaries explicit through objects such as:

- `CapabilityProfile`
- `AvailableAction`
- `BindingState`

The goal is to let agents operate inside structured procurement rules instead of inferring legal or commercial meaning from UI conventions or prose.

## Repository structure

- `docs/`: specifications, architecture notes, and protocol documentation
- `schemas/`: JSON Schemas for core protocol objects
- `openapi/`: draft HTTP transport binding
- `examples/`: example payloads and workflow data

## Key documents

- [Documentation index](docs/index.md)
- [Introduction](docs/introduction.md)
- [Core concepts](docs/core-concepts.md)
- [Architecture](docs/architecture.md)
- [UPP draft specification](docs/upp-draft-spec.md)

## Design principles

- UCP-like architecture, procurement-specific semantics
- discovery before transaction
- small core, extensible edges
- capability negotiation before action
- JSON Schema first
- transport-agnostic semantic core
- secure by default
- compatible with ERP, procurement, and integration systems
- agent-friendly and enterprise-safe
