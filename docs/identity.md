# Identity

The identity model in UPP is intended to support practical enterprise integrations.

It provides enough structure for systems to determine:

- who the buyer is
- who the vendor is
- which legal entity is transacting
- which environment or tenant is involved
- which external IDs map to ERP or procurement systems

## Recommended fields

- stable party ID
- legal name
- display name
- contact email
- billing identifiers
- tax or registration identifiers when needed
- external references such as ERP vendor ID or buyer account ID

## Authentication

UPP is designed to work with common enterprise authentication patterns such as:

- OAuth 2.0
- API keys for server-to-server integrations
- mTLS or signed webhooks where required

The protocol does not depend on exposing interactive agent credentials to trading partners.
