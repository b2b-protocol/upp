# Purchase Orders

The purchase order module defines structured order submission after quote acceptance or another approved commitment flow.

This is the point where the procurement workflow becomes operationally and commercially stronger than discovery or quoting alone.

## What purchase orders are for

A `PurchaseOrder` should answer:

- what the buyer is committing to purchase
- from which vendor
- under which commercial terms
- whether approval has occurred
- what the current execution status is

In a discovery-first architecture, a purchase order should usually be traceable back to:

- an `Intent`
- optionally an `RFQ`
- optionally a `Quote`

That lineage matters for auditability and downstream reconciliation.

## PurchaseOrder object

A `PurchaseOrder` typically includes:

- buyer and vendor identifiers
- optional `intentId`
- optional `rfqId`
- optional `quoteId`
- ordered line items
- shipping or service fulfillment details
- billing references
- approval status
- totals
- payment terms
- `BindingState`
- optional extensions

Unlike discovery results or many quotes, a purchase order should usually represent a stronger commercial state.

That is why `BindingState` should remain explicit rather than being inferred only from `status`.

## Lifecycle

The current draft uses these statuses:

- `draft`
- `submitted`
- `accepted`
- `rejected`
- `partially_fulfilled`
- `fulfilled`
- `cancelled`

Status answers where the order is in its operational lifecycle.

`BindingState` answers how commercially binding the state is.

Those are related but not identical concepts.

## Relationship to approvals

A purchase order is one of the clearest places where approval semantics matter.

Depending on policy:

- a PO may require human approval before submission
- a PO may be auto-generated from an approved quote
- a PO may be created directly from a discovery flow only where delegated authority exists

UPP should make that boundary legible through:

- `ApprovalStatus`
- `AvailableAction`
- `BindingState`

## Interoperability rule

Vendors may translate UPP purchase orders into:

- ERP-native payloads
- EDI documents
- supplier portal actions
- partner-specific APIs

behind their own integration boundary without changing the external UPP contract.

That is part of the coexistence model, not a deviation from the protocol.
