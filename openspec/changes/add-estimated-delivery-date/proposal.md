## Why

The product owner asked: "Agora precisamos mostrar também a data prevista de
entrega." The delivery-status endpoint returns only `orderNumber` and `status`;
clients cannot see when an order is expected to arrive.

## What Changes

- The successful response of `GET /api/orders/{orderNumber}/delivery-status`
  gains a third field with the estimated delivery date.
- The sample orders gain an estimated delivery date each.
- Error responses, validation, `404` and `405` behaviour do not change.

Proposed defaults, pending the product owner's answers below:

- Field name `estimatedDeliveryDate`, a date string `YYYY-MM-DD` (no time, no
  time zone).
- The date is stored with each sample order, not calculated.
- The field is always present; it is `null` for a `DELIVERED` order.
- Sample data: `1001` `IN_TRANSIT` `2026-10-15`, `1002` `PENDING`
  `2026-10-20`, `1003` `DELIVERED` `null`.

## Open Questions

1. **Delivered orders**: should a `DELIVERED` order show the estimate it had,
   the actual delivery date (a different field), nothing (`null`), or omit the
   field? Proposed: `null`.
2. **Unknown estimate**: can a `PENDING` order have no estimate yet? If so,
   `null` or omit the field? Proposed: `null`; no sample order exercises it.
3. **Format and precision**: date only, or date and time / time window? Which
   time zone? Proposed: date only, `YYYY-MM-DD`.
4. **Source**: a fixed value per order, or calculated (e.g. from a shipping
   date plus a lead time)? Proposed: fixed per order in the sample data.
5. **Field name**: proposed `estimatedDeliveryDate`.

This change must not be approved until these are answered or the defaults are
accepted.

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
  "exactly two fields".
