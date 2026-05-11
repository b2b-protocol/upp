# Signatures

UPP should adopt the UCP baseline for HTTP message signing.

The goal is simple:

- prove who sent a message
- prove the message was not changed in transit
- reduce replay and relay risks for agent-mediated procurement actions

## Baseline

For HTTP-based transports, UPP should use:

- RFC 9421 HTTP Message Signatures
- RFC 9530 `Content-Digest`
- JWK for public signing keys

This applies to:

- REST
- MCP over streamable HTTP

## Profile requirements

Signing keys should be published in the party profile at:

- `/.well-known/upp`

The profile should expose a `signingKeys` array of JWK public keys.

At minimum, implementations should support verifying:

- `ES256`

Support for:

- `ES384`

may be added where both parties support it.

## What signatures protect

The signature layer should protect against:

- impersonation
- tampering
- replay across endpoints
- method or path confusion

## Request signing guidance

For state-changing HTTP requests, implementations should sign components such as:

- `@method`
- `@authority`
- `@path`
- `@query` when present
- `upp-agent` when present
- `idempotency-key` for state-changing requests
- `content-digest` when a body is present
- `content-type` when a body is present

For UPP, the profile advertisement header should be:

- `UPP-Agent`

with a profile parameter such as:

- `UPP-Agent: profile="https://platform.example/.well-known/upp"`

## Response signing guidance

Responses carrying commercial or workflow state should also be signed where the transport allows it.

This is especially important for:

- discovery responses
- quotes
- approvals
- purchase order acknowledgements
- invoice status responses

## MCP transport

UPP should follow the same MCP transport signing pattern as the UCP baseline when MCP is carried over streamable HTTP.

That means:

- the MCP request is a normal HTTP request
- the JSON-RPC body is covered by `Content-Digest`
- `Signature-Input` and `Signature` authenticate the HTTP envelope
- `UPP-Agent` and `Idempotency-Key` work the same way they do for REST

Example signed MCP request:

```http
POST /mcp HTTP/1.1
Host: business.example.com
Content-Type: application/json
UPP-Agent: profile="https://platform.example/.well-known/upp"
Idempotency-Key: 550e8400-e29b-41d4-a716-446655440000
Content-Digest: sha-256=:RK/0qy18MlBSVnWgjwz6lZEWjP/lF5HF9bvEF8FabDg=:
Signature-Input: sig1=("@method" "@authority" "@path" "content-digest" "content-type" "upp-agent" "idempotency-key");keyid="platform-2026"
Signature: sig1=:MEUCIQDXyK9N3p5Rt...:

{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{"name":"create_rfq","arguments":{"id":"rfq_123"}}}
```

No JSON canonicalization is required. `Content-Digest` binds the raw JSON-RPC body bytes to the signature.

## Raw body rule

`Content-Digest` should be computed over raw body bytes.

Intermediaries must not reserialize JSON bodies after signing, because that would break verification.

## Error handling

Signature verification errors should use protocol-level error codes consistently across REST and MCP bindings.

Signature-specific errors:

| Code | HTTP | Meaning |
| --- | --- | --- |
| `signature_missing` | `401` | Required signature header or field is missing |
| `signature_invalid` | `401` | Signature verification failed |
| `key_not_found` | `401` | Signing key ID was not found in the signer's profile |
| `digest_mismatch` | `400` | Body digest did not match `Content-Digest` |
| `algorithm_unsupported` | `400` | Signature algorithm is not supported |

Profile-related errors:

| Code | HTTP | Meaning |
| --- | --- | --- |
| `invalid_profile_url` | `400` | Profile URL is malformed or uses an invalid scheme |
| `profile_unreachable` | `424` | Signer profile could not be fetched |
| `profile_not_trusted` | `403` | Signer profile is not trusted by local policy |

Replay protection should remain a business-layer concern driven by idempotency keys, not a signature-layer error category.

Example REST error:

```http
HTTP/1.1 401 Unauthorized
Content-Type: application/json

{
  "code": "signature_invalid",
  "content": "Request signature verification failed for key kid=platform-2026"
}
```

Example MCP error:

```json
{
  "jsonrpc": "2.0",
  "id": 42,
  "error": {
    "code": -32000,
    "message": "Signature verification failed",
    "data": {
      "code": "signature_invalid",
      "content": "Signature verification failed for key kid=platform-2026"
    }
  }
}
```

## Key rotation

UPP should follow the same practical rotation model as UCP:

1. Publish a new key in `signingKeys`
2. Start signing with the new key
3. Accept both old and new keys during a grace period
4. Remove the old key after the grace period

## Current recommendation

For UPP at this stage:

- require profile-published signing keys for signed transports
- treat HTTP signatures as the preferred permissionless trust mechanism
- keep API key, OAuth2, and mTLS as allowed deployment choices
- sign state-changing requests and important responses
