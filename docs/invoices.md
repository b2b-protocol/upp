# Invoices

The invoice module supports structured invoice exchange and matching.

An `Invoice` typically includes:

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

The initial draft uses a lightweight matching model:

- `matched`
- `mismatch`
- `pending_review`
