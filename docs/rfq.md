# RFQ

The RFQ module defines request-for-quote exchange between buyers and vendors.

An `RFQ` typically includes:

- buyer identity
- requested items or service lines
- quantity and unit details
- delivery location
- requested delivery window
- payment term preference
- required response deadline
- optional attachments or references
- optional extensions

A `Quote` typically includes:

- vendor identity
- quote reference
- quoted line items
- pricing and currency
- validity window
- lead time
- payment terms
- assumptions, exclusions, or notes
- optional extensions

The first draft keeps the RFQ and quote model intentionally small so implementations can exchange structured pricing and fulfillment terms without partner-specific adapters for basic flows.
