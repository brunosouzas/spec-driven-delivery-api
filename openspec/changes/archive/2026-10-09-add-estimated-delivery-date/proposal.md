## Why

The product owner asked: "Agora precisamos mostrar também a data prevista de
entrega." The delivery-status endpoint returns only `orderNumber` and `status`;
clients cannot see when an order is expected to arrive.

## What Changes

- The successful response of `GET /api/orders/{orderNumber}/delivery-status`
  gains a third field with the estimated delivery date.
- The sample orders gain an estimated delivery date each.
- Error responses, validation, `404` and `405` behaviour do not change.

Decided with the product owner (2026-10-09):

- Field name `estimatedDeliveryDate`, a date string `YYYY-MM-DD` (no time, no
  time zone).
- The date is stored with each sample order, not calculated.
- The field is always present. `PENDING` and `IN_TRANSIT` orders always have a
  date; only a `DELIVERED` order has `null`.
- Sample data: `1001` `IN_TRANSIT` `2026-10-15`, `1002` `PENDING`
  `2026-10-20`, `1003` `DELIVERED` `null`.

## Decisions on the open questions

1. **Delivered orders**: `null`. An estimate for something already delivered
   means nothing, and the actual delivery date is a different requirement
   nobody asked for.
2. **Unknown estimate**: not allowed. `PENDING` and `IN_TRANSIT` always carry
   a date.
3. **Format and precision**: date only, `YYYY-MM-DD`.
4. **Source**: fixed per order in the sample data.
5. **Field name**: `estimatedDeliveryDate`.
6. **Breaking change** (raised by the proposal itself): accepted on purpose.
   The previous spec promised "exactly two fields", so clients that reject
   unknown fields will break. For this example, versioning the API would be
   more change than the request asks for.

## Out of Scope

- Actual delivery date, delivery history, tracking events, carrier details.
- Calculating or updating estimates; any new endpoint or query parameter.
- Changes to error responses, validation or supported methods.

## Capabilities

### New Capabilities

None.

### Modified Capabilities

- `delivery-status`: the successful response includes the estimated delivery
  date.

## Impact

- `src/main/mule/delivery-status.xml`: sample data and success body.
- `README.md`: updated curl evidence for the success scenarios.
- Breaking for clients that reject unknown fields: the body no longer has
  "exactly two fields". Accepted by the product owner (decision 6).
