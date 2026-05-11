# Introduction

UPP, the Universal Procurement Protocol, is a procurement-domain interoperability protocol for AI-native B2B procurement.

It follows the same broad architectural posture as UCP:

- machine-readable profiles
- capability negotiation before action
- canonical schemas for semantic meaning
- transport-agnostic core workflows

The difference is domain specialization.

UPP is not a generic commerce protocol.

It is focused on procurement interactions between buyers, vendors, procurement platforms, marketplaces, and LLM-driven agents.

Traditional B2B protocols standardized document transport and partner connectivity. They remain important, but they do not provide a shared machine-readable model for procurement intent, supplier discovery, approval-sensitive actions, and commercially meaningful state across AI agents, buyer systems, vendor systems, and marketplaces.

The current UPP draft is discovery-first. It starts earlier than a traditional procure-to-pay workflow:

- register a source document such as a PDF specification or scope of work
- extract structured procurement intent from that source
- search connected suppliers and marketplaces for matching capabilities and deliverables
- move the matched opportunity into RFQ, ordering, approval, and invoice workflows

The initial UPP draft provides a shared model for:

- publishing and consuming capability profiles
- negotiating supported procurement capabilities and versions
- registering source procurement artifacts
- representing structured procurement intent
- expressing discovery requests and matched results
- exposing searchable supplier offerings
- submitting RFQs and retrieving quotes
- creating purchase orders
- reporting approvals and workflow state
- reconciling invoices and payment terms
- attaching namespaced extensions without breaking interoperability

UPP is also intended to be safe for agent use.

That means the protocol should not rely on freeform text alone to decide what happens next. Instead, it should make approval boundaries, available actions, and commercial binding strength explicit in the objects themselves.

The first version is intentionally narrow. It is meant to define a small set of interoperable procurement objects and workflows before expanding into broader procurement coverage.
