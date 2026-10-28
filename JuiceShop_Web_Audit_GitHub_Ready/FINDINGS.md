# Findings register

- **F01 (High, indicative) - SQL injection:** `/rest/products/search?q=`. Empty search control versus returned catalog when query logic was altered. Fix: parameterized queries.
- **F02 (High, indicative) - IDOR:** `/rest/basket/6` returned UserId 25 and `/rest/basket/7` returned UserId 26 when the request path was changed with the same authentication token in the lab. Fix: server-side per-object authorization.
- **F03 (Medium, indicative) - SQL error disclosure:** malformed search returned `SQLITE_ERROR` and SQL text. Fix: generic client error, protected server logging.
- **Not a finding:** unauthenticated `/rest/admin/application-configuration` provided general application config; restricted data was not established.

All observations refer solely to local training instance. See PDF report and response-only evidence.
