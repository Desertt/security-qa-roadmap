# CP-1.4 — Broken Object Level Authorization (BOLA / IDOR)

## Overview
This checkpoint evaluates object-level access control on the `/users/{userId}` endpoint.

The purpose is to determine whether the API properly enforces authorization
when accessing individual user records by manipulating the `userId` parameter.

This test focuses on identifying potential:

> Broken Object Level Authorization (BOLA)  
> also historically known as IDOR (Insecure Direct Object Reference)

---

## Endpoint Under Test
- **Method:** GET
- **Path:** `/users/{userId}`

---

## Test Scenarios

### T01 — Baseline Access
- Description: Retrieve an existing user using a known ID.
- Request: `GET /users/user_100`
- Expected Result: Successful retrieval
- Expected Status Code: `200 OK`
- Outcome: ✅ Verified via Postman

---

### T01 — Object ID Manipulation
- Description: Modify the `userId` parameter to access another user.
- Request: `GET /users/user_101`
- Expected Result (Secure System):
  - `401 Unauthorized` or `403 Forbidden`
- Actual Result:
  - `200 OK`
  - User data returned successfully
- Outcome: ⚠️ Authorization not enforced

---

### T01 — Nonexistent Object
- Description: Request a user ID that does not exist.
- Request: `GET /users/user_999999`
- Expected Result: Resource not found
- Expected Status Code: `404 Not Found`
- Outcome: ✅ Verified via Postman

---

## Tooling
- Postman (manual execution)
- Environment variable:
  - `base_url`
- No authentication token used during this checkpoint

---

## Evidence
Postman executions confirmed:

- Valid user access returns `200 OK`
- Changing the object ID returns `200 OK`
- Nonexistent ID returns `404 Not Found`

Evidence files:
- `CP-1.4-T01_01_get-user-100_200.png`
- `CP-1.4-T01_02_get-user-101_200.png`
- `CP-1.4-T01_03_get-user-999999_404.png`

No sensitive credentials were persisted.

---

## Findings

The API currently allows unrestricted object-level access.

The server does not verify whether the requesting party is authorized
to access the requested `userId`.

Because authentication is not implemented at this stage,
object-level authorization checks are also absent.

In a production environment, this behavior would indicate:

> Broken Object Level Authorization (BOLA) vulnerability.

---

## Risk (Real-World Scenario)

If authentication were implemented without proper object-level authorization:

- Attackers could enumerate user IDs.
- Attackers could access other users' data.
- Sensitive information could be exposed.

Severity (Production Context): **High**

---

## Security Notes

- The system correctly handles nonexistent objects (`404`).
- However, object-level access control is not enforced.
- Authorization logic tied to authenticated identity is missing.

---

## Recommendation

When authentication is implemented:

1. Extract user identity from token (e.g., JWT subject claim).
2. Compare token identity with requested `userId`.
3. Reject mismatched requests with:
   - `403 Forbidden`
4. Implement centralized authorization middleware.

---

## Conclusion

The `/users/{userId}` endpoint currently does not enforce object-level authorization.

This is expected within the lab environment (authentication not yet implemented),
but would constitute a critical vulnerability in production systems.

---

## Status

**CP-1.4 — T01 COMPLETED**

- Baseline Access ✅  
- Object ID Manipulation ⚠️  
- Nonexistent ID Handling ✅  