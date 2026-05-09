# Purchase Orders

The purchase order module standardizes order submission after quote acceptance or direct purchase intent.

A `PurchaseOrder` should contain:

- buyer and vendor identifiers
- optional source RFQ or quote references
- ordered line items
- shipping or service fulfillment details
- billing references
- approval status
- totals
- payment terms
- optional extensions

## Lifecycle

Recommended first-pass statuses:

- `draft`
- `submitted`
- `accepted`
- `rejected`
- `partially_fulfilled`
- `fulfilled`
- `cancelled`

## Interoperability rule

If a vendor must convert UPP purchase orders into EDI, ERP-native payloads, or partner-specific APIs, that translation should happen behind the vendor boundary without changing the UPP-facing contract.
