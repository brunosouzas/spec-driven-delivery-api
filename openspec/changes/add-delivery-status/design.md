## Design

No technical decision between real alternatives. The application uses one HTTP
listener and a DataWeave variable holding the sample orders in place of a
database: `1001` `IN_TRANSIT`, `1002` `PENDING`, `1003` `DELIVERED`. It
validates the order number, looks it up as a string, and maps the result to the
responses in the spec. Internal structure (number of flows, error handlers) is
the implementer's choice.
