# MCP Binding

UPP should treat MCP as an agent access binding over the same procurement semantics used by REST and other transports.

MCP is not the authority layer.

UPP objects remain the authority for:

- workflow state
- approval boundaries
- commercial binding strength
- capability negotiation

## Design rule

An MCP server that exposes UPP should:

- advertise a `CapabilityProfile`
- expose the same canonical UPP schemas used by other bindings
- return UPP objects as tool results or resources
- respect `AvailableAction` and `BindingState`
- avoid inventing transport-only workflow semantics

An MCP server should not:

- bypass approval rules because the caller is a model
- replace structured UPP objects with freeform text-only responses
- introduce hidden state transitions outside the protocol contract

## Transport model

The preferred MCP transport is streamable HTTP.

When UPP is exposed through MCP over HTTP:

- the JSON-RPC message is the HTTP body
- `Content-Digest` covers the raw JSON-RPC body bytes
- `Signature-Input` and `Signature` authenticate the HTTP envelope
- `UPP-Agent` advertises the caller profile URL
- `Idempotency-Key` applies to state-changing tool calls

For signature and trust rules, see:

- [Signatures](signatures.md)

## Canonical tool surface

The first MCP binding should expose a minimal, action-oriented tool set.

Recommended canonical tool names:

- `upp.register_source_artifact`
- `upp.create_intent`
- `upp.get_intent`
- `upp.run_discovery_request`
- `upp.get_discovery_response`
- `upp.create_rfq`
- `upp.get_quote`
- `upp.create_purchase_order`
- `upp.get_approval_status`
- `upp.submit_invoice`

These tool names are intentionally close to the underlying UPP object lifecycle.

## Worked example: signed discovery request

The example below shows a signed MCP call for `upp.run_discovery_request`.

The transport envelope is ordinary HTTP.

The MCP request body is JSON-RPC.

The tool result is a canonical `DiscoveryResponse`.

### Signed MCP request

```http
POST /mcp HTTP/1.1
Host: supplier-network.example
Content-Type: application/json
UPP-Agent: profile="https://buyer-platform.example/.well-known/upp"
Idempotency-Key: 4df7bf84-5d54-4e5b-a74d-d6e0f5d60271
Content-Digest: sha-256=:fVYQn+v8M7Qh9R4qX9sJmPjE1zQz4QG6m5fK7v8J2a0=:
Signature-Input: sig1=("@method" "@authority" "@path" "content-digest" "content-type" "upp-agent" "idempotency-key");keyid="buyer-platform-2026"
Signature: sig1=:MEQCIC7mA3x8S9B9iS2V0k4K...:

{"jsonrpc":"2.0","id":"req_1001","method":"tools/call","params":{"name":"upp.run_discovery_request","arguments":{"request":{"specVersion":"0.2.0-draft","id":"query_1001","type":"DiscoveryRequest","intentId":"intent_1001","buyer":{"id":"buyer_globex","name":"Globex Procurement"},"requestedModules":["UPP Discovery","UPP RFQ"],"constraints":[{"kind":"region","operator":"equals","value":"EU","strength":"required"},{"kind":"leadTimeDays","operator":"lte","value":21,"strength":"required"}]}}}}
```

### MCP success response

```json
{
  "jsonrpc": "2.0",
  "id": "req_1001",
  "result": {
    "structuredContent": {
      "specVersion": "0.2.0-draft",
      "id": "response_1001",
      "type": "DiscoveryResponse",
      "requestId": "query_1001",
      "matches": [
        {
          "supplier": {
            "id": "vendor_acme_industrial",
            "name": "Acme Industrial Supply",
            "contactEmail": "integrations@acme.example"
          },
          "offering": {
            "id": "offering_acme_sensors",
            "supplier": {
              "id": "vendor_acme_industrial",
              "name": "Acme Industrial Supply"
            },
            "title": "Industrial sensor deployment package",
            "summary": "Supply of industrial sensors for retrofit and modernization projects.",
            "categories": [
              "industrial-automation",
              "sensors"
            ],
            "deliverables": [
              {
                "id": "offering_deliverable_1",
                "description": "Industrial sensors with accessories",
                "quantity": 250,
                "unit": "each"
              }
            ],
            "serviceRegions": [
              "EU",
              "EEA"
            ]
          },
          "fitScore": 0.93,
          "bindingState": {
            "state": "indicative",
            "binding": "non_binding",
            "notes": "Discovery result is informative only and not yet a commercial commitment."
          },
          "availableActions": [
            {
              "type": "send_rfq",
              "label": "Send RFQ to supplier",
              "targetType": "Offering",
              "targetId": "offering_acme_sensors",
              "requiresApproval": true
            },
            {
              "type": "request_clarification",
              "label": "Ask supplier for installation lead-time details",
              "targetType": "Offering",
              "targetId": "offering_acme_sensors",
              "requiresApproval": false
            }
          ],
          "explanations": [
            {
              "code": "category_match",
              "summary": "Supplier offering matches industrial automation sensor demand.",
              "matchedFields": [
                "category",
                "deliverables",
                "region"
              ]
            }
          ]
        }
      ]
    }
  }
}
```

### What this example shows

- the MCP caller signs the HTTP envelope, not a custom MCP-only signature object
- the `UPP-Agent` header points to the caller profile used for trust and capability lookup
- the tool argument carries a canonical `DiscoveryRequest`
- the tool result returns a canonical `DiscoveryResponse`
- `availableActions` and `bindingState` tell the model what it may do next and how seriously to treat the result

## Tool behavior

### `upp.register_source_artifact`

Purpose:

- register a PDF, spreadsheet, email package, or extracted text artifact for downstream procurement reasoning

Expected input:

- source metadata
- content reference or inline content
- provenance fields

Expected output:

- a `SourceArtifact`

### `upp.create_intent`

Purpose:

- create or persist a normalized procurement `Intent`

Expected input:

- source artifact references
- extracted deliverables
- constraints
- delivery and timing signals

Expected output:

- an `Intent`

### `upp.get_intent`

Purpose:

- read a previously created `Intent`

Expected input:

- `intentId`

Expected output:

- an `Intent`

### `upp.run_discovery_request`

Purpose:

- execute a procurement discovery search across connected suppliers, marketplaces, or indexes

Expected input:

- an `Intent` reference or embedded `DiscoveryRequest`

Expected output:

- a `DiscoveryResponse`

### `upp.get_discovery_response`

Purpose:

- read a previously created `DiscoveryResponse`

Expected input:

- `discoveryResponseId`

Expected output:

- a `DiscoveryResponse`

### `upp.create_rfq`

Purpose:

- create an `RFQ` from an approved discovery result or buyer-selected supplier

Expected input:

- intent reference
- selected supplier or offering
- line items or requested deliverables
- response deadline

Expected output:

- an `RFQ`

### `upp.get_quote`

Purpose:

- read a `Quote` or current quote state

Expected input:

- `quoteId`

Expected output:

- a `Quote`

### `upp.create_purchase_order`

Purpose:

- create a `PurchaseOrder` from an approved quote or direct commitment flow

Expected input:

- quote or intent reference
- order lines
- buyer and vendor references

Expected output:

- a `PurchaseOrder`

### `upp.get_approval_status`

Purpose:

- fetch current approval state for a workflow object

Expected input:

- target object type
- target object ID

Expected output:

- an `ApprovalStatus`

### `upp.submit_invoice`

Purpose:

- submit an `Invoice` into the procurement flow

Expected input:

- purchase order or fulfillment reference
- invoice fields

Expected output:

- an `Invoice`

## Canonical resource surface

UPP objects should also be exposed as readable MCP resources where that is useful for agent context.

Recommended resource URIs:

- `upp://profile`
- `upp://intents/{id}`
- `upp://discovery-responses/{id}`
- `upp://quotes/{id}`
- `upp://purchase-orders/{id}`
- `upp://approvals/{targetType}/{id}`
- `upp://invoices/{id}`

Resources are especially useful when an agent needs to inspect state without triggering mutation.

## Approval and action rules

MCP tool availability should not be inferred only from tool registration.

The protocol should be the deciding layer:

- `AvailableAction` tells the agent which actions are currently allowed
- `requiresApproval` tells the agent whether human approval is required
- `BindingState` tells the agent whether the current commercial state is exploratory or binding

Example guidance:

- `upp.run_discovery_request` is usually safe before approval
- `upp.create_rfq` may require approval depending on buyer policy
- `upp.create_purchase_order` should usually require approval unless delegated authority has already been granted
- `upp.submit_invoice` may be allowed to vendors but still subject to matching and validation policy

## Capability negotiation

An MCP-exposed UPP server should still negotiate capabilities through `CapabilityProfile`.

That means:

- the server advertises supported services and capabilities
- the caller profile is discoverable through `UPP-Agent`
- the server should only allow tool flows compatible with the negotiated capability intersection

MCP does not replace capability negotiation. It only provides an invocation surface for it.

## Error model

For transport-level signing and profile trust failures, MCP should use the same protocol error codes as REST.

Important examples:

- `signature_missing`
- `signature_invalid`
- `key_not_found`
- `digest_mismatch`
- `algorithm_unsupported`
- `invalid_profile_url`
- `profile_unreachable`
- `profile_not_trusted`

MCP responses should carry those protocol codes inside JSON-RPC error data.

Workflow and policy failures should also remain structured.

Examples:

- approval required
- capability not supported
- object not in a valid state for the requested action

## Current recommendation

For the first UPP MCP binding:

- keep the tool set small and procurement-specific
- use MCP for agent access, not for protocol semantics
- return canonical UPP objects rather than prose whenever possible
- require signed streamable HTTP for state-changing calls in untrusted environments
- let `AvailableAction` and `BindingState` govern safe next steps
