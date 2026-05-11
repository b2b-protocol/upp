# Invoices

The invoice module supports structured invoice exchange and matching after a commercial commitment has already been made.

This is a downstream workflow layer.

It should stay aligned with the same discovery-first and audit-oriented architecture as the rest of UPP.

## What invoices are for

An `Invoice` should answer:

- who is billing whom
- which prior procurement object the invoice relates to
- what line items and totals are being charged
- what the payment terms are
- whether the invoice matches prior workflow state cleanly

An invoice should usually be traceable back to:

- a `PurchaseOrder`
- optionally a `Quote`
- optionally an upstream `Intent`

## Invoice object

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

The important architectural principle is that invoice exchange is not isolated from earlier workflow stages.

It should remain connected to prior commercial state so reconciliation, review, and audit are possible.

## Matching

The current draft uses a lightweight matching model:

- `matched`
- `mismatch`
- `pending_review`

This keeps the invoice layer small while still making downstream workflow state machine-readable.

It also gives agents and procurement systems a clear distinction between:

- invoices that align with prior workflow state
- invoices that need review
- invoices that conflict with prior commitments

## Relationship to payment terms

Invoices often restate or operationalize `PaymentTerms`, but they should not silently replace earlier commercial context.

Where terms differ from upstream quote or order expectations, the mismatch should remain detectable.

## Coexistence rule

Invoice implementations may map UPP invoice objects into:

- ERP invoice records
- AP automation platforms
- e-invoicing networks
- EDI invoice documents

without changing the external UPP contract.

That coexistence model is important because invoice processing is one of the areas where legacy systems are most entrenched.
