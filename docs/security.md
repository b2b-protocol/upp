# Security

UPP is designed for enterprise procurement workflows, which means automation must stay bounded and auditable.

## Security principles

- explicit schemas over free-form commands
- least privilege access
- authenticated counterparties
- versioned and validated payloads
- clear approval boundaries
- auditable workflow transitions

## Guidance informed by MCP security lessons

MCP is useful as a design reference for discoverable capabilities, resources, and bounded interfaces. It is also a reminder that AI integrations can become unsafe when a system treats model output as executable authority.

UPP implementations should:

- validate every payload against schema before processing
- avoid arbitrary tool execution patterns
- never map untrusted agent output directly to shell or ERP admin actions
- separate read, write, and approval permissions
- require explicit approval transitions for high-risk actions
- log extensions and unknown fields for review

## Recommended controls

- signed webhook verification
- idempotency keys for mutating operations
- replay protection for event delivery
- approval checkpoints for order submission and invoice acceptance
- environment separation between sandbox and production
