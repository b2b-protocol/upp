# Capability Discovery

Capability discovery is the entry point for UPP integrations.

Before a buyer system, procurement platform, or agent submits procurement data, it should be able to discover:

- supported protocol version
- supported modules
- supported document types
- accepted currencies and locales
- authentication requirements
- approval behavior
- quote expiry rules
- purchase order constraints
- invoice submission method
- vendor extensions

## Why it matters

In many B2B integrations, these details are documented through onboarding documents, email threads, or partner-specific implementation guides. UPP expresses them as machine-readable capability data.

## Discovery flow

1. Client fetches the vendor capability document.
2. Client validates the document against the capability schema.
3. Client adapts its workflow to the vendor's supported modules and constraints.
4. Client submits RFQs, orders, or invoices only for supported interactions.

## Minimal requirement

A UPP implementation should expose a capability document at a stable URL such as:

- `GET /.well-known/upp-capabilities`
- or `GET /upp/capabilities`
