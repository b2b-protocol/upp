# Roadmap

This roadmap describes the current draft work plan for UPP.

Yes, there is now an explicit planning structure.

The work is organized into milestones rather than only broad phases.

## Planning principles

The roadmap follows a few rules:

- stabilize the semantic core before broadening coverage
- prioritize discovery-first procurement before deep downstream integrations
- keep UCP as the architectural baseline
- make agent safety explicit at each stage
- add enterprise coexistence patterns only after the core protocol is coherent

## Milestone 1: Protocol foundation

Goal:

- define a coherent procurement-domain protocol identity

Scope:

- draft specification
- core concepts
- architecture
- capability discovery model
- canonical capability names
- initial roadmap and research notes

Exit criteria:

- UPP is clearly positioned as UCP-like in architecture and procurement-specific in semantics
- roles, services, capabilities, and transport posture are defined
- `CapabilityProfile` and `/.well-known/upp` are part of the baseline

Status:

- substantially in progress

## Milestone 2: Discovery-first semantic core

Goal:

- make upstream procurement reasoning machine-usable

Scope:

- `SourceArtifact`
- `Intent`
- `Constraint`
- `Offering`
- `DiscoveryRequest`
- `DiscoveryResponse`
- `MatchReason`
- `AvailableAction`
- `BindingState`

Exit criteria:

- source material, buyer intent, supplier fit, next actions, and binding strength are modeled explicitly
- discovery is separated from capability negotiation
- agent-safe workflow boundaries are part of the object model

Status:

- substantially in progress

## Milestone 3: Canonical schemas and examples

Goal:

- turn the semantic model into machine-checkable artifacts

Scope:

- JSON Schemas
- example payloads
- self-describing payload direction
- annotation model for future `upp-schema` tooling

Exit criteria:

- canonical schemas exist for the current draft objects
- examples cover discovery and transaction flows
- schema composition and request/response resolution direction are documented

Status:

- substantially in progress

## Milestone 4: Transport bindings

Goal:

- expose the same semantics through practical runtime bindings

Scope:

- REST binding
- MCP binding
- signature model
- profile-advertised services and bindings

Exit criteria:

- REST and MCP are documented as bindings rather than semantic authorities
- signed streamable HTTP MCP is defined at the transport level
- transport docs point back to canonical schemas and profiles

Status:

- in progress

## Milestone 5: Transaction workflow hardening

Goal:

- tighten the downstream commercial flow after discovery

Scope:

- RFQ and quote semantics
- purchase order semantics
- invoice and payment term semantics
- approval status behavior

Exit criteria:

- downstream transaction objects are aligned with the discovery-first architecture
- approval-sensitive actions and binding state remain explicit through the commercial flow
- module docs read as protocol modules, not feature notes

Status:

- in progress

## Milestone 6: Reference implementation layer

Goal:

- make the protocol executable, testable, and integrator-friendly

Scope:

- reference server
- validation tooling
- conformance test direction
- eventing model for status propagation

Exit criteria:

- a minimal reference implementation can publish a profile and execute core flows
- validation and example-driven testing exist
- event-oriented status updates have a documented direction

Status:

- in progress

## Milestone 7: Enterprise coexistence and regulated procurement support

Goal:

- make UPP credible in real procurement environments

Scope:

- ERP adapter examples
- EDI mapping guidance
- evidence and certificate exchange refinements
- centralized purchasing, framework-style sourcing, and dynamic supplier participation patterns
- coexistence guidance for e-procurement suites

Exit criteria:

- UPP can be explained as an interoperability layer that coexists with current systems
- regulated and public-sector concerns have a clear extension path
- evidence exchange direction is documented without distorting the core protocol

Status:

- planned

## Milestone 8: Governance and ecosystem readiness

Goal:

- make the protocol maintainable beyond the draft

Scope:

- extension registry direction
- partner onboarding guides
- versioning and compatibility guidance
- release and conformance expectations

Exit criteria:

- vendors and platforms can understand how to adopt and extend the protocol
- capability and extension governance is coherent
- the draft can move toward a more formal release process

Status:

- planned
