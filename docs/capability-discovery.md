# Capability Discovery

Capability discovery is the entry point for interaction negotiation in UPP.

Before a buyer system, procurement platform, marketplace, or agent exchanges procurement objects with another party, it should be able to discover what that party supports and under which constraints.

## What capability discovery answers

Capability discovery answers questions such as:

- which protocol version does this party support
- which profile URLs exist for supported versions
- which services and transport bindings are available
- which capability names and versions are supported
- which auth methods are required
- which workflow constraints or endpoint rules apply

It does not answer whether that supplier can satisfy a specific buying need.

That belongs to the [Discovery](discovery.md) layer.

## Why it matters

In many B2B integrations, these details are distributed across:

- onboarding documents
- email threads
- partner-specific implementation guides
- portal configuration screens

UPP turns that into machine-readable capability data.

This reduces bespoke integration work and makes agentic interaction safer, because the client can negotiate what is actually supported before attempting workflow actions.

## Capability profile

Capability discovery starts from `CapabilityProfile`.

A UPP implementation should expose a profile at a stable well-known URL such as:

- `GET /.well-known/upp`

At minimum, the profile should declare:

- `specVersion`
- `supportedVersions`
- `services`
- `capabilities`
- `bindings`
- `auth`
- `endpoints`
- `constraints`

When signed transports are supported, the profile should also expose public signing keys.

## Discovery and negotiation flow

The expected flow is:

1. The client fetches the business capability profile.
2. The client validates the profile against the canonical capability schema.
3. The client determines the intersection between its own supported capabilities and the business profile.
4. The business selects the active capability set from that intersection.
5. The client adapts its workflow to the negotiated capabilities, bindings, and constraints.
6. The client submits discovery, RFQ, order, or invoice workflows only for supported interactions.

This follows the same broad server-selects shape used in UCP.

## UCP-derived design rules

Following the UCP baseline, UPP should treat profile discovery, capability negotiation, and workflow execution as distinct concerns:

- discovery fetches a business profile from a well-known location
- negotiation computes the usable capability intersection between parties
- transport bindings reference canonical schemas instead of redefining payload shapes inline
- extensions compose onto base schemas rather than forking object models

## Services and capabilities

UPP distinguishes between services and capabilities.

- a service groups related procurement functionality under a shared namespace
- a capability represents a specific independently versioned semantic feature

The first canonical service is:

- `com.b2bprotocol.procurement`

The first canonical capability set is:

- `com.b2bprotocol.procurement.intent`
- `com.b2bprotocol.procurement.discovery`
- `com.b2bprotocol.procurement.offering`
- `com.b2bprotocol.procurement.rfq`
- `com.b2bprotocol.procurement.order`
- `com.b2bprotocol.procurement.invoice`
- `com.b2bprotocol.procurement.approval`

See [Capabilities](capabilities.md) for the normative list and semantics.

## Relationship to transport bindings

Capability discovery is transport-independent.

The same negotiated capability set may then be exposed through:

- REST
- MCP
- A2A
- events

The binding changes how the workflow is invoked.

It should not change what the capability means.
