# Architecture

UPP follows the same broad architectural posture as UCP:

- parties publish machine-readable profiles
- capabilities are negotiated before action
- canonical schemas define semantic meaning
- multiple transport bindings expose the same workflow model

The difference is domain specialization.

UPP applies that architecture to B2B procurement rather than general commerce.

## High-level model

UPP is best understood as four cooperating layers:

1. Semantic core
2. Profile and capability negotiation
3. Procurement workflow objects
4. Transport and integration bindings

Each layer has a distinct job.

## 1. Semantic core

The semantic core defines the protocol-wide rules that do not depend on any one transport or procurement stage.

This layer includes:

- shared object envelope conventions
- identifiers
- timestamps
- parties and role model
- extension rules
- versioning
- approval-sensitive state concepts

The semantic core is where object meaning lives.

This is important because UPP should remain authoritative even when exposed through different integration styles.

REST, MCP, A2A, or event streams may change the invocation surface.

They should not change what an `Intent`, `DiscoveryResponse`, `RFQ`, or `PurchaseOrder` means.

## 2. Profile and capability negotiation

Before action, a party should be able to discover what another party supports.

UPP uses a profile-based model for this:

- a business publishes a `CapabilityProfile`
- the profile is discovered at `/.well-known/upp`
- the profile declares services, capabilities, versions, transport bindings, auth requirements, and constraints
- the calling side may advertise its own profile URL through `UPP-Agent`

Negotiation should follow the same server-selects pattern as UCP:

1. Intersect supported capability names.
2. Intersect mutually supported versions.
3. Prune extensions whose parents are not active.
4. Let the business expose the active capability set for the chosen transport.

This layer answers:

- what can this party do
- which version of that capability can we both use
- which transport binding should we use
- what constraints or auth requirements apply

It does not answer whether a supplier can fulfill a specific buying need.

That belongs to the procurement workflow layer.

## 3. Procurement workflow objects

Once capabilities are negotiated, UPP uses explicit procurement objects to carry workflow state.

The current draft is discovery-first.

That means the protocol starts before RFQ or ordering.

### Discovery-first path

The first workflow arc is:

1. `SourceArtifact`
2. `Intent`
3. `DiscoveryRequest`
4. `DiscoveryResponse`
5. `RFQ`, `PurchaseOrder`, or internal review

This is the architectural shift that matters most for UPP.

Traditional procurement integrations often begin after supplier selection.

UPP begins earlier, when a buyer or agent is still turning source material into structured procurement intent and searching the supplier network.

### Object groups

The current object model can be understood in four groups.

#### Input and intent

- `SourceArtifact`
- `Intent`
- `Constraint`

These objects turn unstructured procurement material into structured machine-usable need.

#### Discovery and matching

- `Offering`
- `DiscoveryRequest`
- `DiscoveryResponse`
- `MatchReason`

These objects support supplier and deliverable discovery before a formal commercial request is sent.

#### Transaction and commitment

- `RFQ`
- `Quote`
- `PurchaseOrder`
- `Invoice`
- `PaymentTerms`

These objects carry the downstream procurement transaction flow once discovery narrows to a real supplier path.

#### Governance and safety

- `ApprovalStatus`
- `AvailableAction`
- `BindingState`
- `Extension`

These objects and concepts make the workflow safe for enterprise and agent use.

### Why explicit safety matters

UPP is designed to be usable by LLMs and software agents.

That means the architecture must separate:

- what exists
- what matches
- what is allowed next
- what is commercially binding

Agents should not infer those boundaries from freeform prose or UI conventions.

They should be present in the protocol objects themselves.

## 4. Transport and integration bindings

UPP is transport-agnostic at the semantic layer.

It should be exposable through different runtime bindings without changing the meaning of the underlying objects.

Current binding directions:

- `REST`
- `MCP`
- `A2A`
- `Events`

### REST

REST is the most direct enterprise integration binding.

Typical uses:

- buyer and supplier system integration
- marketplace APIs
- ERP and procurement-suite adapters

### MCP

MCP is an agent access binding.

It is not the core procurement protocol.

In MCP:

- the server exposes UPP workflows as tools and resources
- UPP objects remain the authority layer for workflow state
- approval and commercial binding should still be decided by UPP objects, not by tool descriptions alone

### A2A

A2A is useful where agent-to-agent interactions or delegated execution patterns are needed.

Even there, UPP should still govern procurement object meaning and allowed transitions.

### Events

Event-driven bindings are useful for:

- approval updates
- quote status changes
- purchase order updates
- invoice status and reconciliation flows
- ERP synchronization

## Coexistence model

UPP is not intended to assume greenfield systems.

It should coexist with:

- ERP systems
- procurement suites
- EDI gateways
- document and invoice platforms
- marketplace connectors
- supplier portals

This means UPP should act as an interoperability layer, not as a replacement mandate for every downstream system.

An implementation may:

- expose UPP externally
- translate into internal ERP or sourcing models
- emit EDI or invoice messages downstream
- preserve the external UPP contract while mapping internally

## Architectural rules

UPP should follow these architectural rules:

- keep the semantic core transport-agnostic
- keep capability negotiation separate from supplier-fit discovery
- reference canonical schemas from bindings rather than redefining payloads inline
- compose extensions onto base capabilities instead of forking object models
- preserve provenance between source material, extracted intent, and downstream transactions
- make agent-facing action boundaries explicit through protocol objects
- distinguish exploratory discovery from binding commercial commitment

## Current recommendation

For the current draft:

- keep UCP as the architectural baseline
- specialize semantics for procurement rather than general commerce
- prioritize discovery-first workflow coverage before broadening transaction scope
- keep MCP as an access binding, not the semantic authority
- make every major workflow stage legible to both enterprise systems and LLM agents
