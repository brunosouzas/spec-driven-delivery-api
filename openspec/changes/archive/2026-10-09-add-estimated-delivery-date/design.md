## Design

No technical decision between real alternatives. Each entry in the existing
DataWeave `orders` variable holds a status and an estimated delivery date
(`null` for `DELIVERED`); the success branch adds `estimatedDeliveryDate` to the
body. The date is kept as a `YYYY-MM-DD` string so it is returned exactly as
stored. Validation, `404` and `405` branches are unchanged.
