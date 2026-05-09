# Purchase Orders

The purchase order module defines order submission after quote acceptance or direct purchase intent.

A `PurchaseOrder` typically includes:

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

The initial draft uses these statuses:

- `draft`
- `submitted`
- `accepted`
- `rejected`
- `partially_fulfilled`
- `fulfilled`
- `cancelled`

## Interoperability rule

Vendors may translate UPP purchase orders into EDI, ERP-native payloads, or partner-specific APIs behind their own integration boundary without changing the external UPP contract.
