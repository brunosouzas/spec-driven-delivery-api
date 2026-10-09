## MODIFIED Requirements

### Requirement: Return delivery status by order number
The API SHALL return the current delivery status of an order when called with
`GET /api/orders/{orderNumber}/delivery-status` for an order number it knows.
The order number is an identifier matched exactly as sent: leading zeros are
part of it and are not removed. A successful response SHALL have status `200`,
header `Content-Type: application/json` and a body with exactly three fields:
`orderNumber` (string, as sent), `status` (one of `PENDING`, `IN_TRANSIT`,
`DELIVERED`) and `estimatedDeliveryDate`, which SHALL be a `YYYY-MM-DD` string
for `PENDING` and `IN_TRANSIT` and SHALL be `null` for `DELIVERED`.

#### Scenario: Known order in transit
- **WHEN** a client sends `GET /api/orders/1001/delivery-status` and order `1001` has status `IN_TRANSIT` and estimated delivery date `2026-10-15`
- **THEN** the API responds `200` with `Content-Type: application/json`
- **AND** the body is `{"orderNumber": "1001", "status": "IN_TRANSIT", "estimatedDeliveryDate": "2026-10-15"}`

#### Scenario: Known order pending
- **WHEN** a client sends `GET /api/orders/1002/delivery-status` and order `1002` has status `PENDING` and estimated delivery date `2026-10-20`
- **THEN** the API responds `200` with `Content-Type: application/json`
- **AND** the body is `{"orderNumber": "1002", "status": "PENDING", "estimatedDeliveryDate": "2026-10-20"}`

#### Scenario: Known order delivered
- **WHEN** a client sends `GET /api/orders/1003/delivery-status` and order `1003` has status `DELIVERED`
- **THEN** the API responds `200` with `Content-Type: application/json`
- **AND** the body is `{"orderNumber": "1003", "status": "DELIVERED", "estimatedDeliveryDate": null}`
