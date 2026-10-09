# Tasks

## 1. Application

- [x] 1.1 Create the Mule 4 project (Java 17, Maven, runtime 4.12.x) with an HTTP listener; verified by `mvn clean package` succeeding
- [x] 1.2 Add the sample orders and the `GET /api/orders/{orderNumber}/delivery-status` endpoint with the responses in the spec; verified by `mvn clean package` succeeding (HTTP behaviour is verified in group 2)

## 2. Evidence

Run the application locally. For each task, record in `README.md` the curl call and the real response (status line, `Content-Type` and body). A task is done only when the captured status and `Content-Type` match its scenario and the JSON body matches it structurally (same field names, types and values; whitespace and field order do not matter).

- [x] 2.1 Scenarios "Known order in transit", "Known order pending" and "Known order delivered"
- [x] 2.2 Scenarios "Order does not exist" and "Leading zeros are a different identifier"
- [x] 2.3 Scenarios "Non-numeric order number" and "Order number longer than 20 digits"
- [x] 2.4 Scenarios "POST on the endpoint" and "Method is checked before the order number"
- [x] 2.5 Run `openspec validate --all --strict`; verified by it passing
