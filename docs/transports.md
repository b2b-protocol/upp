# Transports

UPP is transport-agnostic at the semantic layer.

The core protocol defines objects, capability negotiation, and workflow state. Concrete transport bindings expose those semantics through different runtime interfaces.

## Design rule

All transport bindings should point back to the same canonical UPP schemas and capability profile model.

Transport bindings should:

- expose a `CapabilityProfile`
- identify supported capabilities and versions
- describe available endpoints or invocation surfaces
- preserve the same object meanings across bindings

Transport bindings should not:

- redefine core payload shapes independently
- fork object semantics per transport
- introduce hidden state transitions

## Supported binding kinds

### REST

REST is the most direct enterprise integration binding.

Typical use:

- buyer systems and supplier systems exchange UPP objects over HTTPS
- marketplaces expose discovery and RFQ endpoints
- ERP and procurement platforms use API adapters

Discovery should begin at:

- `GET /.well-known/upp`

The REST binding draft lives in:

- `openapi/upp-api.yaml`

For signed REST exchanges, see:

- [Signatures](signatures.md)

### MCP

MCP is an agent access binding, not the core commerce protocol.

Typical use:

- an agent host connects to an MCP server
- the MCP server exposes UPP workflows as tools or resources
- UPP objects remain the authority layer for workflow state

Example MCP tool surface:

- `register_source_artifact`
- `create_intent`
- `run_discovery_request`
- `create_rfq`
- `fetch_quote`
- `create_purchase_order`

Example MCP resources:

- capability profile
- intent records
- discovery responses
- quote and order status records

The MCP binding draft lives in:

- [MCP Binding](mcp-binding.md)

When MCP is carried over streamable HTTP, the same HTTP signature model should apply to the transport envelope.

In practice this means:

- the JSON-RPC message is the HTTP body
- `Content-Digest` binds that body to the signature
- `UPP-Agent` advertises the caller profile
- `Idempotency-Key` remains the replay-protection input for state-changing calls

### A2A

A2A can be used as a transport for agent-to-agent exchange when an implementation wants conversational or delegated execution behavior on top of UPP semantics.

UPP should still remain the semantic authority for:

- object meaning
- action boundaries
- approval requirements
- commercial state

### Events

Event-driven bindings can be used for status propagation and enterprise integration pipelines.

Typical use:

- approval updates
- quote status changes
- invoice matching events
- downstream ERP synchronization

## Capability negotiation

Capability negotiation should happen independently of the chosen transport.

The minimum shared model is:

- profile discovery from `/.well-known/upp`
- declared services
- declared capability names
- declared capability versions
- declared transport bindings
- declared constraints and auth methods

## Current recommendation

For UPP at this stage:

- keep the semantic core transport-agnostic
- publish a REST binding first
- add an MCP binding profile for LLM and agent access
- keep both bindings pointed at the same schema set
