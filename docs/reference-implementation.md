# Reference Implementation

This document defines the minimal reference implementation shape for UPP.

At this stage, the goal is not a production server.

The goal is a small executable surface that proves the protocol can:

- publish a `CapabilityProfile`
- accept or expose core discovery-first objects
- preserve workflow lineage
- make approval and binding state explicit
- validate payloads against canonical schemas

## Purpose

The reference implementation layer exists to turn the draft from:

- documentation
- schemas
- examples

into something an integrator can execute, test, and reason about.

## Minimal reference server scope

A minimal reference server should implement these capabilities first:

- `com.b2bprotocol.procurement.intent`
- `com.b2bprotocol.procurement.discovery`
- `com.b2bprotocol.procurement.rfq`
- `com.b2bprotocol.procurement.order`
- `com.b2bprotocol.procurement.invoice`
- `com.b2bprotocol.procurement.approval`

It should expose a profile at:

- `GET /.well-known/upp`

It should then provide a minimal end-to-end workflow:

1. Register or expose a `SourceArtifact`
2. Create or retrieve an `Intent`
3. Execute a `DiscoveryRequest`
4. Return a `DiscoveryResponse`
5. Create an `RFQ`
6. Return a `Quote`
7. Create a `PurchaseOrder`
8. Expose `ApprovalStatus`
9. Submit an `Invoice`

## Minimal transport surface

The preferred first reference binding is REST.

It is the simplest way to prove:

- canonical schema usage
- profile discovery
- object lineage
- idempotent mutation behavior

The second reference binding should be MCP.

It should expose the same underlying objects and policy boundaries through tools and resources rather than redefining the workflow.

## Reference behavior rules

A minimal reference implementation should:

- publish a valid `CapabilityProfile`
- return objects using canonical schema shapes
- preserve IDs and cross-object references
- keep discovery separate from capability negotiation
- expose `AvailableAction` in discovery responses
- expose `BindingState` in quote and purchase order flows
- expose `ApprovalStatus` as a separate readable object
- reject unsupported workflows with structured errors

It should not:

- invent hidden workflow transitions
- collapse approval semantics into prose-only responses
- treat MCP as a semantic authority separate from UPP

## Recommended first server behavior

The first reference server can be intentionally simple.

Recommended behavior:

- static or fixture-backed supplier offerings
- deterministic discovery matching
- deterministic quote generation from RFQ inputs
- mock approval state transitions
- fixture-backed invoice matching outcomes

This is enough to prove interoperability before adding complex business logic.

## Persistence model

The first implementation does not need a heavy persistence layer.

A minimal server may use:

- in-memory storage
- fixture-backed storage
- file-backed JSON state

as long as it preserves:

- stable object identifiers
- object lineage
- timestamped state changes

## Validation requirements

The reference implementation should validate:

- `CapabilityProfile`
- request payloads
- response payloads
- self-describing payload metadata where used

Validation should use the canonical schemas in `schemas/`.

When operation-specific request and response views are needed, the implementation should align with the `upp_request` / `upp_response` annotation model described in [Schema Tooling](schema-tooling.md).

## Testable reference flows

The first reference implementation should support these testable flows:

### Flow 1: Discovery-first procurement

1. Client fetches `/.well-known/upp`
2. Client creates or resolves an `Intent`
3. Client executes a `DiscoveryRequest`
4. Server returns `DiscoveryResponse` with `MatchReason`, `AvailableAction`, and `BindingState`

### Flow 2: RFQ from discovery

1. Client selects an allowed `AvailableAction`
2. Client creates an `RFQ`
3. Server returns a `Quote`

### Flow 3: Order and approval

1. Client creates a `PurchaseOrder`
2. Server exposes `ApprovalStatus`
3. Order status progresses independently from approval state

### Flow 4: Invoice and matching

1. Client submits an `Invoice`
2. Server returns invoice state
3. Invoice exposes `matchStatus`

## Implementation recommendation

When code is added, start with:

- one reference REST server
- one validation command or script
- one conformance smoke test path

Do not try to build:

- a full procurement platform
- production authentication orchestration
- advanced ranking or supplier intelligence

The reference layer should prove the protocol, not replace real systems.
