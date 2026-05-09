# RFQ

The RFQ module standardizes request-for-quote exchange.

An `RFQ` should contain:

- buyer identity
- requested items or service lines
- quantity and unit details
- delivery location
- requested delivery window
- payment term preference
- required response deadline
- optional attachments or references
- optional extensions

A `Quote` should contain:

- vendor identity
- quote reference
- quoted line items
- pricing and currency
- validity window
- lead time
- payment terms
- assumptions, exclusions, or notes
- optional extensions

## Goal

The goal is not to model every procurement edge case up front. The goal is to let a buyer-side agent obtain a structured quote without a custom adapter for every vendor.
