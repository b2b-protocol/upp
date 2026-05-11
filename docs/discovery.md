# Discovery

Discovery is the entry point for agent-mediated procurement in UPP.

This is one of the main differences between UPP and older procurement integration patterns.

UPP does not begin only at RFQ, ordering, or invoicing.

It begins earlier, when a buyer, operator, or agent is still turning source material into structured procurement intent and searching the network for viable suppliers and deliverables.

## What discovery is for

The discovery layer answers a different question from capability discovery.

- `CapabilityProfile` answers: how do I interact with this party?
- `Offering` answers: what can this party deliver?
- `DiscoveryRequest` answers: who in the network can fulfill this buyer need?

This distinction matters.

A supplier may support UPP perfectly and still not be a good match for a specific procurement need.

## Discovery flow

The expected discovery-first path is:

1. Register a `SourceArtifact` such as a PDF specification, scope of work, email package, or bill of quantities.
2. Extract or author an `Intent`.
3. Convert that intent into a `DiscoveryRequest`.
4. Execute the request across connected suppliers, marketplaces, or indexes.
5. Receive a `DiscoveryResponse` containing matched suppliers, offerings, fit signals, and next actions.
6. Convert shortlisted matches into RFQs, clarifications, internal review, or direct order flows where policy allows.

## Discovery objects

### SourceArtifact

A `SourceArtifact` represents the original buyer input.

Typical fields:

- source identifier
- filename
- media type
- content hash
- source system
- upload timestamp
- page count

The important architectural property is provenance.

Later workflow objects should be traceable back to the source material they were derived from.

### Intent

An `Intent` is the normalized buyer need.

Typical fields:

- buyer identity
- title and summary
- requested deliverables
- quantities
- delivery location
- required dates
- budget signal
- required certifications
- mandatory and optional constraints

An intent should be stable enough to support discovery and downstream transaction flows without forcing every supplier to parse the original artifact differently.

### DiscoveryRequest

A `DiscoveryRequest` is the network-searchable form of the buyer need.

Typical fields:

- intent reference
- supplier or geography filters
- category filters
- hard constraints
- preferred constraints
- requested response shape

This object is about supplier-fit discovery, not yet commercial commitment.

### DiscoveryResponse

A `DiscoveryResponse` is the result set returned by a supplier, marketplace, or discovery service.

Typical fields:

- supplier matches
- relevant offerings or deliverables
- lead-time and location fit
- commercial signals
- binding state
- constraint mismatches
- confidence or fit score
- match explanations
- available actions with approval semantics

## Agent-safe discovery

Discovery must be safe for agents, not only informative for humans.

That means a response should not just list matches.

It should also make workflow boundaries explicit.

### MatchReason

`MatchReason` explains why a supplier or offering was returned.

This supports:

- buyer review
- agent transparency
- debugging of matching behavior

### AvailableAction

`AvailableAction` tells an agent what it may do next.

Examples:

- request clarification
- send RFQ
- open internal review
- submit order

An action should also indicate whether approval is required before execution.

### BindingState

`BindingState` tells the agent whether the result is:

- exploratory
- indicative
- quoted
- approved
- committed

This prevents discovery results from being mistaken for commercial commitments.

## Relationship to capability discovery

Capability discovery and supplier-fit discovery are distinct.

Capability discovery determines whether a party supports the protocol features needed for a workflow.

Discovery determines whether that party is a good match for a specific procurement need.

Both are needed.

Neither should replace the other.

## Relationship to RFQ

Discovery should narrow the search space before a formal RFQ is sent.

In some implementations, a `DiscoveryResponse` may lead directly to:

- an RFQ
- a commercial clarification request
- a manual buyer review
- a contract-backed order flow

UPP should not leave that entirely as UI logic.

It should let a response enumerate `AvailableAction` objects so an agent can distinguish:

- actions it may take automatically
- actions that require human approval
- actions that are informative but not yet binding
