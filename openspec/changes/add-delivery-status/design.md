## Design

No technical decision between real alternatives. The application follows the
project convention: one HTTP listener, one flow, and a DataWeave variable
holding sample orders (`1001` `IN_TRANSIT`, plus one `PENDING` and one
`DELIVERED`) in place of a database. The flow validates the order number,
looks it up, and maps the result to `200`, `404` or `400` as the spec says.

Path, body and status values follow the proposal's defaults; if the product
owner answers the open questions differently, update the spec before
implementing.
