# Invoices

The invoice module supports structured invoice exchange and matching.

An `Invoice` should contain:

- invoice identifier
- buyer and vendor identifiers
- related purchase order reference
- related quote reference when available
- invoice line items
- tax and total amounts
- payment terms
- due date
- match status
- optional extensions

## Matching

The initial protocol should support at least a lightweight matching model:

- `matched`
- `mismatch`
- `pending_review`

This keeps the first version implementation-friendly while leaving room for richer three-way match logic later.
