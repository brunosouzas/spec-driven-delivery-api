# spec-driven-delivery-api

A small Mule 4 API built with spec-driven development (SDD) using
[OpenSpec](https://github.com/Fission-AI/OpenSpec).

The interesting part is not the API. It is the path from a one-line request to
code:

1. A short, incomplete request.
2. An OpenSpec change (`proposal.md`, `specs/`, `design.md`, `tasks.md`) that
   makes the missing decisions explicit, reviewed before any code exists.
3. Human approval of one commit of that change.
4. An AI agent implementing from the approved artifacts only.
5. Each spec scenario proven with a real call, below.
6. The change archived into `openspec/specs/`, and a second change that evolves
   the API through a delta.

Follow it in the commit history and in `openspec/`.

## Status

The `add-delivery-status` change is implemented and each of its scenarios is
proven with a real call (see [Evidence](#evidence), run on Mule runtime 4.12.3).

## Evidence

Each scenario of `add-delivery-status`, run against the application deployed on a
Mule 4.12.3 runtime with `curl -si`. Status line, `Content-Type` and body are the
real responses.

### Known order in transit

```
$ curl -si http://localhost:8081/api/orders/1001/delivery-status
HTTP/1.1 200 OK
content-type: application/json
content-length: 53

{
  "orderNumber": "1001",
  "status": "IN_TRANSIT"
}
```

### Known order pending

```
$ curl -si http://localhost:8081/api/orders/1002/delivery-status
HTTP/1.1 200 OK
content-type: application/json
content-length: 50

{
  "orderNumber": "1002",
  "status": "PENDING"
}
```

### Known order delivered

```
$ curl -si http://localhost:8081/api/orders/1003/delivery-status
HTTP/1.1 200 OK
content-type: application/json
content-length: 52

{
  "orderNumber": "1003",
  "status": "DELIVERED"
}
```

### Order does not exist

```
$ curl -si http://localhost:8081/api/orders/9999/delivery-status
HTTP/1.1 404 Not Found
content-type: application/json
content-length: 39

{
  "message": "Order 9999 not found"
}
```

### Leading zeros are a different identifier

```
$ curl -si http://localhost:8081/api/orders/01001/delivery-status
HTTP/1.1 404 Not Found
content-type: application/json
content-length: 40

{
  "message": "Order 01001 not found"
}
```

### Non-numeric order number

```
$ curl -si http://localhost:8081/api/orders/abc/delivery-status
HTTP/1.1 400 Bad Request
content-type: application/json
content-length: 44
Connection: close

{
  "message": "Invalid order number: abc"
}
```

### Order number longer than 20 digits

```
$ curl -si http://localhost:8081/api/orders/123456789012345678901/delivery-status
HTTP/1.1 400 Bad Request
content-type: application/json
content-length: 62
Connection: close

{
  "message": "Invalid order number: 123456789012345678901"
}
```

### POST on the endpoint

```
$ curl -si -X POST http://localhost:8081/api/orders/1001/delivery-status
HTTP/1.1 405 Method Not Allowed
content-type: application/json
content-length: 37

{
  "message": "Method not allowed"
}
```

### Method is checked before the order number

```
$ curl -si -X POST http://localhost:8081/api/orders/abc/delivery-status
HTTP/1.1 405 Method Not Allowed
content-type: application/json
content-length: 37

{
  "message": "Method not allowed"
}
```
