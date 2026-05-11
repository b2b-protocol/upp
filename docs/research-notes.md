# Research Notes

These notes capture the main external reference points used for the initial UPP draft.

## UCP

UCP is a useful reference point for agent-mediated commerce flows. Public materials describe interoperability across discovery, buying, payment, and post-purchase workflows, along with implementation patterns such as REST, JSON-RPC, and adjacent protocol ecosystems.

For UPP, the main takeaway is the value of a small interoperable core and explicit workflow primitives rather than one-off vendor integrations.

References:

- <https://ucp.dev/>
- <https://shopify.engineering/UCP>
- <https://www.shopify.com/news/ai-commerce-at-scale>

## MCP

MCP is a useful reference point for capability discovery and schema-driven integration. Its architecture documents define host, client, and server interactions around resources, tools, prompts, and capability negotiation.

For UPP, the relevant patterns are explicit capabilities, schema-oriented interfaces, and bounded interactions. The related security guidance also reinforces input validation and explicit approval boundaries.

References:

- <https://modelcontextprotocol.io/docs/learn/architecture>
- <https://modelcontextprotocol.io/specification/2025-11-25/architecture>
- <https://modelcontextprotocol.io/specification/2025-03-26/server/resources>
- <https://modelcontextprotocol.io/specification/2025-06-18/server/prompts>

## Traditional B2B integration

IBM Sterling and OpenText documentation remains useful for understanding the current enterprise B2B integration landscape. Common themes include document transport, translation, partner onboarding, support for EDI and AS2, managed file transfer, and operational traceability.

For UPP, this supports a coexistence model: improve procurement workflow interoperability without assuming immediate replacement of existing EDI or managed integration stacks.

References:

- <https://www.ibm.com/products/b2b-integrator>
- <https://www.ibm.com/docs/en/b2b-integrator/6.2.1?topic=as2-using>
- <https://www.opentext.com/products/trading-grid>
- <https://www.opentext.com/products/b2b-integration-essentials>

## Public procurement and e-procurement

The SIGMA/OECD public procurement brief on e-procurement is useful as a baseline for how public-sector procurement modernization has traditionally been framed. It emphasizes electronic publication, unrestricted document access, electronic submission, structured evidence handling, e-invoicing, monitoring, interoperability, and end-to-end administrative traceability.

For UPP, the main takeaway is not to become a public-sector tender portal. The more relevant lesson is that serious procurement environments expect structured, auditable digital workflow across the entire lifecycle rather than isolated messaging steps.

Implications for UPP:

- `SourceArtifact` and provenance are important because procurement begins with source documents and auditability matters.
- Discovery and RFQ flows should preserve timestamps, integrity, and clear workflow boundaries.
- Profiles, schemas, and transport bindings should support interoperable exchange rather than vendor-specific portals.
- Structured evidence and certificate exchange will likely matter later, especially for regulated or public-sector procurement flows.
- Invoice and approval handling should remain part of the protocol surface even if discovery is the first product slice.
- UPP should be able to coexist with centralized purchasing bodies, framework agreements, and dynamic supplier systems rather than assuming direct one-to-one sourcing only.

Less relevant parts for UPP:

- UPP should not inherit legacy assumptions that all innovation begins from contracting-authority portals.
- The protocol should not be reduced to notice publication or tender submission mechanics.
- UPP should remain useful for private-sector and agent-mediated procurement, not only public-sector compliance flows.

Reference:

- SIGMA Public Procurement Brief 17, *E-Procurement*, OECD/SIGMA, September 2016

## API/spec ecosystem

JSON Schema and OpenAPI remain practical baselines for machine-readable contracts in web-native systems.

For UPP, this supports starting with JSON Schemas and a draft OpenAPI surface, with room to add event-oriented specifications later if needed.

References:

- <https://json-schema.org/>
- <https://spec.openapis.org/oas/latest.html>
- <https://www.asyncapi.com/en>
