# Schema Tooling

This document describes the schema composition and resolution model UPP should borrow from the UCP baseline.

The goal is not to copy UCP shopping semantics. The goal is to reuse the proven schema mechanics for agent-facing interoperability.

## Why this matters

UPP objects are meant to be used by:

- buyer systems
- supplier systems
- procurement platforms
- LLM agents

That means the protocol needs more than static JSON Schema files. It needs a predictable way to:

- compose extensions onto base schemas
- resolve request or response views for a specific operation
- validate self-describing payloads at runtime

## Proposed pipeline

UPP should adopt the same progressive pipeline shape as `ucp-schema`:

1. `compose`
2. `resolve`
3. `validate`

### Compose

Compose merges the base schema plus negotiated capability extensions into a single schema graph.

Typical use:

- take a root procurement object such as `Intent` or `RFQ`
- add capability-driven extensions via `allOf`
- preserve UPP-specific annotations for later resolution

### Resolve

Resolve turns a composed schema into an operation-specific view.

Typical use:

- request vs response
- create vs read vs update vs complete
- hide fields that must not appear in a client request
- mark fields required only in specific operations

### Validate

Validate checks a payload against the resolved schema.

Typical use:

- validate a self-describing response directly
- validate a request using a fetched profile
- validate an explicit schema in testing or CI

## Self-describing payloads

UPP should support self-describing payloads through top-level `upp` metadata.

Example shape:

```json
{
  "upp": {
    "capabilities": {
      "com.b2bprotocol.discovery.search": [{
        "version": "0.2.0-draft",
        "schema": "https://b2bprotocol.com/schemas/discovery-response.schema.json"
      }]
    }
  },
  "specVersion": "0.2.0-draft",
  "id": "response_1001",
  "type": "DiscoveryResponse"
}
```

This allows a receiver or agent runtime to:

- determine which capability schemas apply
- fetch or resolve them
- compose the correct effective schema
- validate without out-of-band guessing

## Annotation model

UPP should define UPP-specific schema annotations analogous to UCP's request and response visibility rules.

Recommended annotation names:

- `upp_request`
- `upp_response`

Recommended operations:

- `create`
- `read`
- `update`
- `complete`

Recommended values:

- `omit`
- `optional`
- `required`

The first annotated UPP schemas are:

- `schemas/intent.schema.json`
- `schemas/rfq.schema.json`
- `schemas/discovery-response.schema.json`

## Example visibility rule

If an `id` field is server-generated on create but required on update, one source schema can express both:

```json
{
  "type": "string",
  "upp_request": {
    "create": "omit",
    "update": "required"
  },
  "upp_response": "required"
}
```

The resolved schema for request/create omits the field.

The resolved schema for request/update keeps the field and adds it to `required`.

## Extension composition

UPP extensions should compose onto base schemas through `allOf`, not fork base schemas into incompatible variants.

That means:

- one root capability provides the base schema
- extensions declare what they extend
- extension additions should live in `$defs` keyed by the root capability name
- composition should fail if the capability graph is invalid

## Version constraints

UPP should also adopt capability-level version constraints for extensions.

Recommended pattern:

- root profile declares capability names and versions
- extension schemas declare minimum and optional maximum protocol or capability versions
- composition fails if requirements are not met

## What belongs in UPP core vs tooling

UPP core should define:

- object semantics
- capability profile structure
- canonical capability names
- extension rules
- annotation names and allowed values

Tooling should implement:

- compose
- resolve
- validate
- lint
- local schema mapping
- bundling

## Current recommendation

For UPP now:

- keep these mechanics documented as protocol guidance
- do not force every implementation to ship the full pipeline immediately
- design the schema set so a future `upp-schema` tool can implement the same flow cleanly
