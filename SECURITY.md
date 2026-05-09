# Security Policy

UPP is an early-stage protocol draft for enterprise procurement workflows.

Security-sensitive areas include:

- authentication and authorization
- approval workflow boundaries
- schema validation and input handling
- webhook signing and replay protection
- mappings into ERP, procurement, or financial systems

When reviewing changes, prefer:

- explicit schemas over free-form instructions
- least privilege access patterns
- idempotent mutating operations
- auditable status transitions
- clear separation between discovery, execution, and approval
