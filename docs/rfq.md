# RFQ

The RFQ module defines structured request-for-quote exchange between buyers and vendors.

In UPP, RFQ is downstream of discovery.

That means an `RFQ` should usually be traceable back to:

- an `Intent`
- a `DiscoveryResponse`
- a buyer-approved supplier or offering selection

An RFQ is the point where supplier-fit exploration begins to turn into a commercially meaningful request.

## What RFQ is for

The RFQ layer answers:

- what exactly is the buyer asking this supplier to quote
- by when must the supplier respond
- which quantities, locations, or constraints are commercially relevant
- which upstream discovery or intent context led to this request

This is different from discovery.

Discovery asks who may be a fit.

RFQ asks a specific supplier to price and respond to a defined need.

## RFQ object

An `RFQ` should carry enough structured context for a supplier to respond without re-parsing the original source material from scratch.

Typical fields:

- buyer identity
- vendor identity
- optional `intentId`
- optional `discoveryResponseId`
- requested items or service lines
- quantity and unit details
- delivery location
- requested delivery window
- payment term preference
- required response deadline
- optional attachments or references
- optional extensions

An RFQ should remain clearly non-binding until the workflow moves into an accepted quote, approved order, or other commitment step.

## Quote object

A `Quote` is the vendor response to an RFQ.

Typical fields:

- vendor identity
- optional buyer identity
- RFQ reference
- optional upstream intent or discovery references
- quoted line items
- pricing and currency
- validity window
- lead time
- payment terms
- totals
- `BindingState`
- assumptions, exclusions, or notes
- optional extensions

The quote is where commercial meaning sharpens.

That is why `BindingState` matters here.

Not every quote should be interpreted as equally binding by an agent or procurement platform.

## Workflow boundaries

The RFQ module should preserve the distinction between:

- exploratory discovery
- supplier-specific commercial response
- approved commercial commitment

That means:

- discovery results should not be mistaken for quotes
- quotes should not be mistaken for approved commitments
- agents should not infer RFQ authority from the existence of a supplier match alone

## Relationship to approvals

Some organizations will allow an agent or operator to create RFQs automatically.

Others will require human approval before a supplier can be contacted formally.

UPP should support both models by keeping approval-sensitive actions explicit through `AvailableAction`, `ApprovalStatus`, and policy rather than burying that distinction in UI behavior.

## Current draft recommendation

For the current draft:

- keep RFQ and quote objects small but commercially meaningful
- preserve lineage from intent and discovery into RFQ and quote objects
- treat quotes as structured commercial responses, not generic messages
- keep binding strength explicit through `BindingState`
