# Project Guidance

This repository is for B2B Protocol and UPP, the Universal Procurement Protocol.

## Working rules

- Research before inventing. Prefer primary sources and direct documentation over generic summaries.
- Keep the scope minimal. Do not expand UPP into a broad universal standard without a concrete implementation need.
- Avoid speculative claims about adoption, compatibility, or industry support unless they are directly sourced.
- Do not present internal planning language as public-facing documentation.
- Avoid hype, crypto language, and vague AI marketing language.
- Prefer practical implementation artifacts: Markdown specs, JSON Schema, OpenAPI, examples, and reference code.

## Core positioning

- B2B Protocol is the company or ecosystem brand.
- UPP is the protocol or specification layer.
- UPP is an interoperability layer for AI-native B2B procurement.
- UPP does not assume immediate replacement of EDI, AS2, or existing ERP and procurement infrastructure.
- UPP focuses on procurement workflow interoperability rather than arbitrary tool execution.

## Required research anchors

Use these as the baseline references for protocol-related work:

- UCP:
  - <https://ucp.dev/>
  - Google and Shopify materials related to UCP or agentic commerce
- MCP:
  - <https://modelcontextprotocol.io/>
  - MCP architecture, tools, resources, and security guidance
- Traditional B2B integration:
  - IBM Sterling
  - OpenText
  - Cleo
  - EDI, AS2, SFTP, FTPS, RosettaNet, RNIF
- API/spec ecosystem:
  - JSON Schema
  - OpenAPI
  - AsyncAPI when event-driven scope is relevant

## Current minimum scope

- Vendor capability discovery
- RFQ request and response
- Purchase order creation
- Approval workflow status
- Invoice matching
- Payment terms
- Buyer and vendor identity
- Vendor-specific extensions

## Editorial rules

- Be technical, enterprise-grade, and developer-first.
- Prefer precise language over visionary language.
- Mark assumptions clearly.
- If a statement is a design proposal rather than an established fact, phrase it as a proposal.
- Keep README and landing-page copy concise and public-facing.
- Put rationale, comparisons, and source notes in dedicated docs rather than mixing them into the front page.
