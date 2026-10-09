## Why

The product owner asked for an API that, given an order number, returns the
delivery status of that order ("Precisamos de uma API que, dado o número do
pedido, retorne o status da entrega."). No such API exists yet.

## What Changes

- New Mule 4 API with one read-only endpoint that takes an order number and
  returns the delivery status of that order.
- Delivery data lives in a DataWeave variable that stands in for a database
  (project convention; no external systems).
- Responses for an unknown order number and for a malformed order number.

Proposed defaults (marked *proposal*, to confirm with the product owner; see
Open Questions):

- *proposal*: `GET /api/orders/{orderNumber}/delivery-status`.
- *proposal*: response body `{"orderNumber": "...", "status": "..."}`, JSON.
- *proposal*: status values `PENDING`, `IN_TRANSIT`, `DELIVERED`.
- *proposal*: order number is 1 to 20 digits; anything else returns `400`.
- *proposal*: unknown order number returns `404`.

## Out of Scope

- Creating, updating or cancelling orders or deliveries.
- Delivery history, tracking events, carrier details or estimated dates.
- Authentication, authorisation, rate limiting.
- Integration with any real order or logistics system.
- API specification publishing (RAML/OAS in Exchange), deployment.

## Open Questions

1. **Order number format**: digits only? Length? Prefixes such as `PED-123`?
2. **Status values**: which statuses exist, and in Portuguese or English?
3. **Response content**: only the status, or also fields like last update time?
4. **Unknown order**: `404`, or `200` with an "unknown" status?
5. **Consumers and access**: who calls the API, and does it need authentication?
6. **Path and naming**: is there an existing URL convention to follow?

## Capabilities

### New Capabilities

- `delivery-status`: look up the delivery status of an order by order number.

### Modified Capabilities

None.

## Impact

- New Mule 4 application (Java 17, Maven, runtime 4.9.x) with one HTTP
  listener and one flow; sample data in a DataWeave variable.
- `README.md` gains curl calls and real responses for each scenario.
