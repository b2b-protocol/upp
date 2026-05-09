# Research Notes

These notes capture the external reference points used to shape the initial UPP draft.

## UCP

UCP positions itself as a common language for agentic commerce across discovery, buying, and post-purchase flows. Its public materials emphasize:

- interoperable building blocks rather than one-off integrations
- support for REST and JSON-RPC transports
- compatibility with adjacent standards such as MCP, A2A, and AP2
- extensibility and business control

Implication for UPP: procurement needs the same interoperability mindset, but with modules centered on RFQs, purchase orders, approvals, invoices, and enterprise system boundaries rather than consumer checkout.

References:

- <https://ucp.dev/>
- <https://shopify.engineering/UCP>
- <https://www.shopify.com/news/ai-commerce-at-scale>

## MCP

MCP standardizes host, client, and server interactions around resources, tools, prompts, and capability negotiation. It is a useful model for:

- explicit capability discovery
- schema-oriented interfaces
- separation between clients and servers
- resource addressing and bounded interactions

Its security guidance also reinforces the need to validate inputs and outputs and to avoid injection-prone execution patterns.

Implication for UPP: adopt the discoverability and schema discipline, but do not model procurement as arbitrary tool execution. High-risk workflow transitions should remain explicit, validated, and policy-bound.

References:

- <https://modelcontextprotocol.io/docs/learn/architecture>
- <https://modelcontextprotocol.io/specification/2025-11-25/architecture>
- <https://modelcontextprotocol.io/specification/2025-03-26/server/resources>
- <https://modelcontextprotocol.io/specification/2025-06-18/server/prompts>

## Traditional B2B integration

IBM Sterling and OpenText product documentation still reflects the current operational center of gravity for enterprise B2B exchange:

- document transport and translation
- partner onboarding
- support for EDI, AS2, managed file transfer, and API hybrids
- reliability, traceability, and secure delivery

Implication for UPP: do not position the protocol as “replacing EDI.” Position it as a modern workflow interoperability layer that can sit in front of or alongside existing integration platforms.

References:

- <https://www.ibm.com/products/b2b-integrator>
- <https://www.ibm.com/docs/en/b2b-integrator/6.2.1?topic=as2-using>
- <https://www.opentext.com/products/trading-grid>
- <https://www.opentext.com/products/b2b-integration-essentials>

## API/spec ecosystem

JSON Schema and OpenAPI remain the most practical baseline for machine-readable contracts in web-native systems.

Implication for UPP: start with JSON Schemas and a draft OpenAPI surface. Expand later only if event-driven flows require an AsyncAPI layer.

References:

- <https://json-schema.org/>
- <https://spec.openapis.org/oas/latest.html>
- <https://www.asyncapi.com/en>
