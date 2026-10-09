# Tasks

## 1. Application

- [ ] 1.1 Create the Mule 4 project (Java 17, Maven, runtime 4.9.x) with an HTTP listener; verify with `mvn clean package`
- [ ] 1.2 Add the sample orders as a DataWeave variable and the `GET /api/orders/{orderNumber}/delivery-status` flow returning `200`, `404` and `400` as specified; verify with `mvn clean package`

## 2. Evidence

- [ ] 2.1 Run the application and record in `README.md` the curl call and real response for "Known order"; verify the response matches the spec
- [ ] 2.2 Same for "Order does not exist"
- [ ] 2.3 Same for "Non-numeric order number"
- [ ] 2.4 Run `openspec validate --all --strict`; verify it passes
