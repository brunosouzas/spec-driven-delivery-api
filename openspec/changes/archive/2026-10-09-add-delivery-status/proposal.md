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

Decided with the product owner (2026-10-09):

- `GET /api/orders/{orderNumber}/delivery-status`.
- Successful response body `{"orderNumber": "...", "status": "..."}`, JSON.
  Only these two fields. Errors return JSON with only `message`.
- Order number is an identifier matched exactly as sent; leading zeros are kept.
- `POST`, `PUT`, `PATCH` and `DELETE` on the endpoint return `405`, before any
  order-number check. `HEAD` and `OPTIONS` are out of scope.
- Status values `PENDING`, `IN_TRANSIT`, `DELIVERED`.
- Order number is 1 to 20 digits; anything else returns `400`.
- A well-formed order number that does not exist returns `404`: the order is
  the resource named by the URL, and the resource was not found. `400` stays
  for a request that is malformed.

## Out of Scope

- Creating, updating or cancelling orders or deliveries.
- Delivery history, tracking events, carrier details or estimated dates.
- Authentication, authorisation, rate limiting.
- Integration with any real order or logistics system.
- API specification publishing (RAML/OAS in Exchange), deployment.

## Decisions on the open questions

1. **Order number format**: digits only, 1 to 20.
2. **Status values**: `PENDING`, `IN_TRANSIT`, `DELIVERED`, in English.
3. **Response content**: only `orderNumber` and `status` on success; only
   `message` on error.
4. **Unknown order**: `404`. The owner asked whether a missing order is a
   business error (`400`) or "page not found" (`404`); without a spec, the
   implementer would have picked one silently.
5. **Consumers and access**: local example, no authentication (out of scope).
6. **Path and naming**: the proposed path.

## Capabilities

### New Capabilities

- `delivery-status`: look up the delivery status of an order by order number.

### Modified Capabilities

None.

## Impact

- New Mule 4 application (Java 17, Maven, runtime 4.12.x) with one HTTP
  listener; sample data in a DataWeave variable.
- `README.md` gains curl calls and real responses for each scenario.
- Other paths under the application are out of scope and keep Mule's default
  behaviour.
