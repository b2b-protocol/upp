# Capability Discovery

Capability discovery is the first workflow in UPP.

Before an agent or buyer system submits procurement data, it should be able to discover:

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

In legacy B2B integration, these details are often buried in onboarding documents, email threads, or partner-specific implementation guides. UPP makes them machine-readable.

## Discovery flow

1. Client fetches the vendor capability document.
2. Client validates the document against the capability schema.
3. Client adapts its workflow to the vendor's supported modules and constraints.
4. Client submits RFQs, orders, or invoices only for supported interactions.

## Minimal requirement

Every UPP implementation should expose a capability document at a stable URL such as:

- `GET /.well-known/upp-capabilities`
- or `GET /upp/capabilities`
