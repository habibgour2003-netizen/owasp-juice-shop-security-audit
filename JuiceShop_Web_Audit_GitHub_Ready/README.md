# OWASP Juice Shop - Web Security Audit (Local Lab)

**Author:** Habib GUERMANE · Engineering student in Networks & Cybersecurity (EMI)  
**Date:** 28 October 2026 · **Target:** OWASP Juice Shop running on `http://localhost:3000`

## Overview

This repository documents an authorized, local-only security assessment of the intentionally vulnerable OWASP Juice Shop training application. Burp Suite Community Edition was used to inspect HTTP traffic, replay requests, and compare control/test responses.

> This is a **training lab**, not an audit of a real organization. The screenshots are cropped to show HTTP responses only. Authentication tokens, session cookies, and secrets must never be committed.

## Findings

| ID | Finding | Outcome | Suggested severity |
|---|---|---|---|
| F01 | SQL Injection in product search (`q`) | Confirmed by empty baseline vs unexpected product list | High (context-dependent) |
| F02 | Broken access control / IDOR (`/rest/basket/{id}`) | Reading basket 7, UserId 26, while using the account-A request previously used for basket 6, UserId 25 | High (context-dependent) |
| F03 | Improper error handling / SQL information exposure | HTTP 500 returns `SQLITE_ERROR` and internal SQL statement fragment | Medium (context-dependent) |

### Non-finding

`/rest/admin/application-configuration` returned HTTP 200 without credentials, but no sensitive-only information or intended access restriction was established. **Not reported as broken access control.**

## Evidence (response-only captures)

| Evidence | What it demonstrates |
|---|---|
| [01_sql_error_response.png](docs/evidence/01_sql_error_response.png) | SQL error details in an HTTP 500 response |
| [02_search_baseline_empty.png](docs/evidence/02_search_baseline_empty.png) | Unknown product term yields an empty result set |
| [03_sqli_products_returned.png](docs/evidence/03_sqli_products_returned.png) | Modified search expression returns products |
| [04_basket_A_user25.png](docs/evidence/04_basket_A_user25.png) | Basket 6 returned UserId 25 |
| [05_basket_B_user26.png](docs/evidence/05_basket_B_user26.png) | Basket 7 returned UserId 26 |

## Audit report

**[Read the French PDF audit report](reports/Rapport_Audit_Web_Juice_Shop_Habib_Guermane.pdf)**

## Method summary

1. Run OWASP Juice Shop locally and connect a Burp Suite Community browser.
2. Observe product search and cart requests via **Proxy > HTTP history**.
3. Reproduce requests in **Repeater**, comparing a benign control request against a single changed input.
4. Compare search result sets and logged-in access to different basket IDs.
5. Document evidence, expected authorization behavior, scope limitations, impact, and remediation.

## Remediation

- **SQL injection:** use parameterized queries and regression tests.
- **IDOR:** enforce server-side object ownership checks for every basket request.
- **Error disclosure:** log detailed exceptions on the server, but return generic client-facing errors.

## Scope and limitations

- Only the local intentionally vulnerable training application was tested.
- No extensive data access, account modification, persistence, production infrastructure, or third-party systems were tested.
- Severity labels are indicative and do not reflect a production risk assessment.
- The IDOR conclusion relies on the same token being preserved during the documented Repeater path change; the token value is intentionally excluded from public evidence.
- No fixes to the actual Juice Shop application were deployed or verified.

## Portfolio description

*Performed a local OWASP Juice Shop web-security assessment using Burp Suite, documenting SQL injection, IDOR, and SQL error disclosure, with reproducible evidence and remediation guidance.*
