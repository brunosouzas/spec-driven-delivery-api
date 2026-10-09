# Tasks

## 1. Application

- [ ] 1.1 Add the estimated delivery date to the sample orders and `estimatedDeliveryDate` to the success body in `src/main/mule/delivery-status.xml`; verified by `mvn clean package` succeeding (HTTP behaviour is verified in group 2)

## 2. Evidence

Run the application locally. For each task, update `README.md` with the curl call and the real response (status line, `Content-Type` and body). A task is done only when the captured status and `Content-Type` match its scenario and the JSON body matches it structurally (same field names, types and values; whitespace and field order do not matter).

- [ ] 2.1 Scenarios "Known order in transit", "Known order pending" and "Known order delivered"
- [ ] 2.2 Re-run one call per unchanged error scenario (`404`, `400`, `405`) and confirm the responses are unchanged
- [ ] 2.3 Run `openspec validate --all --strict`; verified by it passing
