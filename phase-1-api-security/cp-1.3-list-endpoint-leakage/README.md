# CP-1.3 — List Endpoint Leakage (JWT Authentication)

## Overview
This checkpoint evaluates access control enforcement on the `/users` list endpoint
by testing three different authentication scenarios using JWT-based authorization.

The goal is to ensure that the endpoint is not unintentionally exposed and that
authorization is handled consistently and securely.

---

## Endpoint Under Test
- **Method:** GET
- **Path:** `/users`

---

## Test Scenarios

### T01 — No Token
- Description: Request sent without any Authorization header
- Expected Result: Access denied
- Expected Status Code: `401 Unauthorized` (or `403 Forbidden`)
- Outcome: ✅ Verified via Postman

---

### T02 — Invalid Token
- Description: Request sent with a malformed or invalid JWT
- Expected Result: Access denied
- Expected Status Code: `401 Unauthorized`
- Outcome: ✅ Verified via Postman

---

### T03 — Valid Token
- Description: Request sent with a valid JWT obtained via login flow
- Expected Result: Authorized access
- Expected Status Code: `200 OK`
- Outcome: ✅ Verified via Postman  
- Additional Validation:
  - Response format is JSON
  - Response body returned successfully

---

## Tooling
- Postman (manual execution)
- JWT-based authentication
- Environment variables used for:
  - `base_url`
  - `valid_token`

---

## Evidence
Postman test executions were completed successfully for all three scenarios.
Due to Postman UI limitations during the session, execution screenshots
(evidence) could not be exported.

Validation was confirmed through:
- HTTP status codes
- Postman test assertions
- Manual inspection of responses

> Note: No sensitive information (tokens or user data) was persisted or committed.

---

## Security Notes
- The `/users` endpoint is protected against unauthenticated access.
- Invalid or missing tokens do not expose sensitive information.
- Behavior differs correctly between authorized and unauthorized requests.
- No sensitive fields (passwords, hashes, secrets) were observed in responses.

---

## Conclusion
The `/users` list endpoint enforces JWT-based authorization correctly.
Unauthorized access attempts are blocked, and valid tokens grant access as expected.

This checkpoint confirms that there is no list endpoint leakage related to
authentication misconfiguration.

---

## Status
**CP-1.3 — COMPLETED**

- T01 — No Token ✅
- T02 — Invalid Token ✅
- T03 — Valid Token ✅

### T04 — Expired Token — GET /users
- Expected Result: 401 Unauthorized
- Actual Result: 200 OK
- Status: ❌ Failed (Security Finding)

#### Finding
Requests made with an expired JWT are still accepted by the `/users` endpoint.
The backend does not appear to enforce the `exp` claim during token validation.

This behavior allows continued access with expired tokens and may lead to
unauthorized data exposure.

#### Risk
- Authentication bypass via expired tokens
- Session lifetime not enforced
- Increased attack window if a token is leaked

#### Recommendation
Ensure JWT expiration (`exp`) is validated during token verification and
expired tokens are rejected with `401 Unauthorized`.
