## Purpose

Lets a client look up the current delivery status of an order by its order number.

## ADDED Requirements

### Requirement: Return delivery status by order number
The API SHALL return the current delivery status of an order when called with
`GET /api/orders/{orderNumber}/delivery-status` for an order number it knows.
The response SHALL have status `200`, header `Content-Type: application/json`
and a body with `orderNumber` (string) and `status` (one of `PENDING`,
`IN_TRANSIT`, `DELIVERED`).

#### Scenario: Known order
- **WHEN** a client sends `GET /api/orders/1001/delivery-status` and order `1001` has status `IN_TRANSIT`
- **THEN** the API responds `200` with `Content-Type: application/json`
- **AND** the body is `{"orderNumber": "1001", "status": "IN_TRANSIT"}`

### Requirement: Unknown order number
The API SHALL respond `404` with a JSON body containing a `message` field when
the order number is well formed but no order has it.

#### Scenario: Order does not exist
- **WHEN** a client sends `GET /api/orders/9999/delivery-status` and no order `9999` exists
- **THEN** the API responds `404` with `Content-Type: application/json`
- **AND** the body is `{"message": "Order 9999 not found"}`

### Requirement: Malformed order number
The API SHALL respond `400` with a JSON body containing a `message` field when
the order number is not 1 to 20 digits.

#### Scenario: Non-numeric order number
- **WHEN** a client sends `GET /api/orders/abc/delivery-status`
- **THEN** the API responds `400` with `Content-Type: application/json`
- **AND** the body is `{"message": "Invalid order number: abc"}`
