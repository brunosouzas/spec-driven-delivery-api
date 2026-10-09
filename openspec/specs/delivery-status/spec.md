# delivery-status Specification

## Purpose
Lets a client look up the current delivery status of an order by its order number.

## Requirements

### Requirement: Return delivery status by order number
The API SHALL respond `200` with header `Content-Type: application/json` to
`GET /api/orders/{orderNumber}/delivery-status` for an order number it knows.
The order number is an identifier matched exactly as sent: leading zeros are
part of it and are not removed.

#### Scenario: Known order in transit
- **WHEN** a client sends `GET /api/orders/1001/delivery-status` and order `1001` has status `IN_TRANSIT` and estimated delivery date `2026-10-15`
- **THEN** the API responds `200` with `Content-Type: application/json`
- **AND** the body is `{"orderNumber": "1001", "status": "IN_TRANSIT", "estimatedDeliveryDate": "2026-10-15"}`

#### Scenario: Known order pending
- **WHEN** a client sends `GET /api/orders/1002/delivery-status` and order `1002` has status `PENDING` and estimated delivery date `2026-10-20`
- **THEN** the API responds `200` with `Content-Type: application/json`
- **AND** the body is `{"orderNumber": "1002", "status": "PENDING", "estimatedDeliveryDate": "2026-10-20"}`

### Requirement: Successful response body
A successful response body SHALL have exactly three fields: `orderNumber`
(string, as sent), `status` (one of `PENDING`, `IN_TRANSIT`, `DELIVERED`) and
`estimatedDeliveryDate`, a `YYYY-MM-DD` string for `PENDING` and `IN_TRANSIT`
and `null` for `DELIVERED`.

#### Scenario: Known order delivered
- **WHEN** a client sends `GET /api/orders/1003/delivery-status` and order `1003` has status `DELIVERED`
- **THEN** the API responds `200` with `Content-Type: application/json`
- **AND** the body is `{"orderNumber": "1003", "status": "DELIVERED", "estimatedDeliveryDate": null}`

### Requirement: Error responses
Every error response SHALL have header `Content-Type: application/json` and a
body with exactly one field, `message` (string).

#### Scenario: Error body shape
- **WHEN** the API responds `400`, `404` or `405` to a request on `/api/orders/{orderNumber}/delivery-status`
- **THEN** the response has `Content-Type: application/json`
- **AND** the body has only the field `message`

### Requirement: Unknown order number
The API SHALL respond `404` with body `{"message": "Order {orderNumber} not found"}`
when the order number is well formed but no order has it.

#### Scenario: Order does not exist
- **WHEN** a client sends `GET /api/orders/9999/delivery-status` and no order `9999` exists
- **THEN** the API responds `404`
- **AND** the body is `{"message": "Order 9999 not found"}`

#### Scenario: Leading zeros are a different identifier
- **WHEN** a client sends `GET /api/orders/01001/delivery-status`, order `1001` exists and no order `01001` exists
- **THEN** the API responds `404`
- **AND** the body is `{"message": "Order 01001 not found"}`

### Requirement: Malformed order number
The API SHALL respond `400` with body `{"message": "Invalid order number: {orderNumber}"}`
when the order number does not match `[0-9]{1,20}` (ASCII digits only, 1 to 20).

#### Scenario: Non-numeric order number
- **WHEN** a client sends `GET /api/orders/abc/delivery-status`
- **THEN** the API responds `400`
- **AND** the body is `{"message": "Invalid order number: abc"}`

#### Scenario: Order number longer than 20 digits
- **WHEN** a client sends `GET /api/orders/123456789012345678901/delivery-status` (21 digits)
- **THEN** the API responds `400`
- **AND** the body is `{"message": "Invalid order number: 123456789012345678901"}`

### Requirement: Unsupported method
The API SHALL respond `405` with body `{"message": "Method not allowed"}` to
`POST`, `PUT`, `PATCH` and `DELETE` on `/api/orders/{orderNumber}/delivery-status`,
whatever the order number: the order number is validated and looked up only for
`GET`. `HEAD`, `OPTIONS` and other methods are out of scope.

#### Scenario: POST on the endpoint
- **WHEN** a client sends `POST /api/orders/1001/delivery-status`
- **THEN** the API responds `405`
- **AND** the body is `{"message": "Method not allowed"}`

#### Scenario: Method is checked before the order number
- **WHEN** a client sends `POST /api/orders/abc/delivery-status`
- **THEN** the API responds `405`
- **AND** the body is `{"message": "Method not allowed"}`
