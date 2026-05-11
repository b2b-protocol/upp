# Core Concepts

UPP is a procurement-domain interoperability protocol for platforms, businesses, marketplaces, and agents.

It follows the same basic architectural posture as UCP:

- entities publish machine-readable profiles
- capabilities are negotiated before action
- canonical schemas define object meaning
- multiple transport bindings can expose the same semantics

The difference is domain focus.

UCP is a general commerce baseline.

UPP specializes that model for B2B procurement, especially where LLM agents and procurement systems need to work from structured intent, searchable offerings, approval boundaries, and commercially meaningful workflow state.

## Terminology

Throughout this documentation:

- `Platform` means any entity that consumes procurement capabilities
- `Business` means any entity that exposes procurement capabilities

These are capability-flow roles, not company-type labels.

A procurement suite can act as a platform.

A supplier can act as a business.

A marketplace can act as both.

## High-level goal

UPP is intended to let one party declare procurement needs and another party expose procurement capabilities in a shared machine-readable language.

Its primary goal is to enable:

- platforms to discover and use procurement capabilities without bespoke integrations
- businesses to expose structured procurement functionality once and serve many compatible platforms
- agents to operate inside explicit workflow and approval boundaries rather than improvising state transitions from freeform text

## Roles and participants

UPP is centered on the interaction between capability consumers and capability providers, with procurement-specific participant roles layered on top.

### Platform

A `Platform` is any entity that consumes capabilities exposed by another party.

Examples:

- AI procurement agents
- procurement suites
- ERP adapters
- sourcing marketplaces
- internal buying portals

Responsibilities:

- discover capability profiles
- negotiate active capabilities
- invoke tools, endpoints, or workflows within those negotiated boundaries
- act on behalf of a buyer, operator, or delegated system principal

### Business

A `Business` is any entity that exposes capabilities to another party.

Examples:

- suppliers
- distributors
- service providers
- marketplaces exposing seller inventory or services

Responsibilities:

- publish a `CapabilityProfile`
- declare supported services, capabilities, and transport bindings
- process procurement workflow requests
- return structured state and explicit allowed next actions

### Buyer

A `Buyer` is the procurement principal expressing need.

A buyer may act through a platform, a human operator, or an autonomous agent.

Typical responsibilities:

- provide source material
- review extracted intent
- approve or reject commercially meaningful actions

### Vendor

A `Vendor` is a supplier-side procurement participant that exposes offerings and may accept discovery, RFQ, order, or invoice workflows.

## Core protocol constructs

UPP revolves around three architectural constructs inherited from the UCP approach and specialized for procurement:

- `Capabilities`
- `Extensions`
- `Services`

These define how procurement functionality is named, negotiated, and invoked.

### Capabilities

Capabilities are standalone, independently versioned procurement features that a business declares it supports.

They are the semantic units a platform can discover and use.

Each capability:

- uses reverse-domain naming
- is versioned independently
- is advertised in `CapabilityProfile.capabilities`
- may be activated only when both sides support it

Examples in the current draft:

- `com.b2bprotocol.procurement.intent`
- `com.b2bprotocol.procurement.discovery`
- `com.b2bprotocol.procurement.offering`
- `com.b2bprotocol.procurement.rfq`
- `com.b2bprotocol.procurement.order`
- `com.b2bprotocol.procurement.invoice`
- `com.b2bprotocol.procurement.approval`

### Extensions

Extensions optionally augment a base capability.

They should follow the same UCP-style model:

- declare their parent capability using `extends`
- compose onto base schemas rather than replacing them
- activate only when both parties support them and their parent capability is active

This keeps the protocol small while allowing domain-specific or vendor-specific additions.

Example extension directions:

- sustainability scoring on discovery results
- sector-specific qualification evidence
- vendor-specific commercial terms

### Services

Services group related procurement functionality under a shared namespace.

In the current draft, the first canonical service is:

- `com.b2bprotocol.procurement`

A service says what procurement functionality exists in that vertical.

Transport bindings say how it is accessed.

The same service may be exposed through:

- `REST`
- `MCP`
- `A2A`
- `Events`

## Capability profile

A `CapabilityProfile` is the entry point for discovery.

It is the UPP analogue of the UCP profile and is published at:

- `/.well-known/upp`

At minimum, it should declare:

- supported protocol versions
- supported services
- supported capabilities and versions
- transport bindings
- auth requirements
- signing keys when signed transports are supported
- workflow endpoints or invocation surfaces

This lets discovery, capability negotiation, and trust bootstrap from one machine-readable profile.

## Discovery and negotiation

UPP uses a profile-based discovery model.

Every business publishes a `CapabilityProfile`.

When HTTP is used, a calling platform advertises its own profile URL with:

- `UPP-Agent`

Capability negotiation should follow the same server-selects shape as UCP:

1. Intersect supported capability names.
2. Select mutually compatible versions.
3. Prune extensions whose parent capabilities are not active.
4. Let the business expose the active set through the chosen transport binding.

The result is a minimal mutually supported procurement surface.

This matters because an agent should never assume a workflow exists just because the object model exists in the abstract.

## Discovery-first procurement objects

UPP distinguishes between source procurement material, extracted buyer intent, and supplier-facing discovery data.

### SourceArtifact

A `SourceArtifact` captures an uploaded artifact such as a PDF specification, bill of quantities, spreadsheet, email package, or extracted text bundle.

It records provenance, file metadata, and references used during later extraction and audit.

### Intent

An `Intent` is the normalized buyer need derived from a `SourceArtifact` or structured form input.

It describes:

- what is being purchased
- required deliverables
- quantities
- locations
- timelines
- commercial signals
- constraints

### Constraint

A `Constraint` is a structured hard or soft requirement attached to an `Intent` or `DiscoveryRequest`.

Examples:

- geography
- certification
- lead time
- budget ceiling
- payment method

### Offering

An `Offering` is a search-friendly supplier description of what can be delivered.

It is distinct from a capability profile.

The capability profile says how to interact with a business.

The offering says what that business can actually provide.

### DiscoveryRequest

A `DiscoveryRequest` is a network-searchable request derived from an `Intent`.

It asks who can fulfill a need and under which constraints before a formal RFQ is sent.

### DiscoveryResponse

A `DiscoveryResponse` returns:

- matched suppliers
- relevant offerings
- fit signals
- constraints
- next available actions

It can include `MatchReason` records so an agent or buyer can understand why a result matched.

## Workflow objects

The current draft defines these core workflow objects:

- `SourceArtifact`
- `Intent`
- `Constraint`
- `Offering`
- `DiscoveryRequest`
- `DiscoveryResponse`
- `MatchReason`
- `AvailableAction`
- `BindingState`
- `RFQ`
- `Quote`
- `PurchaseOrder`
- `Invoice`
- `ApprovalStatus`
- `PaymentTerms`
- `Extension`

## Agent-facing safety concepts

UPP is intended to be usable by LLMs and agents, which means workflow safety must be explicit in the protocol.

### AvailableAction

An `AvailableAction` tells an agent what it may do next.

It should identify:

- the action type
- the target resource
- whether approval is required

This prevents models from inferring workflow legality from prose or UI convention alone.

### BindingState

A `BindingState` tells an agent how seriously to treat the current commercial state.

Examples:

- `exploratory`
- `indicative`
- `quoted`
- `approved`
- `committed`

This helps separate discovery, negotiation, and binding commitment.

## Extension model

Extensions use namespaced keys, for example:

- `com.b2bprotocol.procurement.discovery.sustainability`
- `com.b2bprotocol.procurement.rfq.qualification`
- `com.vendorname.custom_pricing`

Unknown extensions must be safely ignored unless explicitly marked as required by negotiated capability or object contract.

## Versioning

UPP currently uses:

- `specVersion` for the protocol draft version
- `schemaVersion` where schema release tracking is useful

Capability versions may evolve independently from the top-level protocol version.

As with UCP, compatibility should be managed per capability rather than assuming every feature changes in lockstep.
