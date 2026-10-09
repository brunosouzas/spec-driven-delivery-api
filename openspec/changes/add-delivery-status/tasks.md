# Tasks

## 1. Application

- [ ] 1.1 Create the Mule 4 project (Java 17, Maven, runtime 4.9.x) with an HTTP listener; verified by `mvn clean package` succeeding
- [ ] 1.2 Add the sample orders and the `GET /api/orders/{orderNumber}/delivery-status` endpoint with the responses in the spec; verified by `mvn clean package` succeeding (HTTP behaviour is verified in group 2)

## 2. Evidence

Run the application locally. For each task, record in `README.md` the curl call and the real response (status line, `Content-Type` and body). A task is done only when the captured status, `Content-Type` and body match its scenario exactly.

- [ ] 2.1 Scenarios "Known order in transit", "Known order pending" and "Known order delivered"
- [ ] 2.2 Scenarios "Order does not exist" and "Leading zeros are a different identifier"
- [ ] 2.3 Scenarios "Non-numeric order number" and "Order number longer than 20 digits"
- [ ] 2.4 Scenario "POST on the endpoint"
- [ ] 2.5 Run `openspec validate --all --strict`; verified by it passing
