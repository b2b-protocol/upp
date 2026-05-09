# Extensions

UPP uses namespaced extensions so implementers can move faster without fragmenting the base protocol.

## Rules

- Every extension key must be namespaced.
- Base objects must remain valid without vendor extensions unless the capability document says otherwise.
- Unknown optional extensions should be ignored safely.
- Required extensions must be declared in the capability document.

## Examples

- `com.b2bprotocol.procurement.rfq`
- `com.b2bprotocol.procurement.purchase_order`
- `com.vendorname.custom_pricing`

## Design intent

Extensions are for edge logic such as:

- custom pricing rules
- procurement policy metadata
- category-specific attributes
- regional compliance data

They are not an excuse to replace the base contract with opaque blobs.
