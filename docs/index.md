# UPP Documentation

This directory contains the current draft documentation for UPP, the Universal Procurement Protocol.

UPP is a procurement-domain interoperability protocol built in the same general spirit as UCP, but specialized for B2B procurement workflows and LLM-capable agents.

The key idea is simple:

- profiles publish what a party supports
- capabilities are negotiated before action
- canonical objects define workflow meaning
- multiple transport bindings can expose the same semantics

UPP then applies that model to procurement-specific problems such as source artifact intake, intent formation, supplier discovery, RFQ exchange, approvals, orders, and invoices.

## Start here

- [Introduction](introduction.md)
- [Architecture](architecture.md)
- [Core concepts](core-concepts.md)
- [UPP draft specification](upp-draft-spec.md)

## Protocol modules

- [Discovery](discovery.md)
- [Capability discovery](capability-discovery.md)
- [Capabilities](capabilities.md)
- [Transports](transports.md)
- [MCP binding](mcp-binding.md)
- [Schema tooling](schema-tooling.md)
- [Signatures](signatures.md)
- [RFQ](rfq.md)
- [Purchase orders](purchase-orders.md)
- [Invoices](invoices.md)
- [Identity](identity.md)
- [Extensions](extensions.md)

## Supporting material

- [Roadmap](roadmap.md)
- [Research notes](research-notes.md)
- [Security](security.md)

## Related artifacts

- The JSON Schemas in the repository `schemas/` directory define the current object models for capability profiles, discovery resources, RFQs, quotes, purchase orders, invoices, and approvals.
- The OpenAPI draft in `openapi/upp-api.yaml` defines the current HTTP surface for discovery, capability lookup, and core procurement resources.
- The example payloads in the repository `examples/` directory provide a reference capability profile, a discovery-first workflow example, and an RFQ-to-invoice workflow example.
