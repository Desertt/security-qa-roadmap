[🇬🇧 English](README.md) | [🇹🇷 Türkçe](README.tr.md)

# CP-1.3 — List Endpoint Authentication Matrix

## Goal

Evaluate authentication enforcement on the `GET /users` list endpoint across multiple JWT states:

- no token
- invalid token
- valid token
- expired token

## Endpoint Under Test

- **Method:** GET
- **Path:** `/users`

## Test Matrix

### T01 — No Token

**Expected**

- access denied
- typically `401 Unauthorized` or `403 Forbidden`

**Committed evidence**

- `evidence/CP-1.3_T01_no_token_list_exposed_200.png`

**Actual captured behavior**

- evidence filename records `200`

**Result**

⚠️ Security finding — the captured run indicates list access without a token.

---

### T02 — Invalid Token

**Expected**

- `401 Unauthorized`

**Committed evidence**

- `evidence/CP-1.3_T02_invalid_token_list_200.png`

**Actual captured behavior**

- evidence filename records `200`

**Result**

⚠️ Security finding — invalid-token access was not rejected in the captured run.

---

### T03 — Valid Token

**Expected**

- `200 OK`
- JSON response

**Committed evidence**

- `evidence/CP-1.3_T03_valid_token_get_users_200.png`

**Result**

✅ Valid-token access returned the expected success status.

---

### T04 — Expired Token

**Expected**

- `401 Unauthorized`

The committed README history records:

- **Actual:** `200 OK`
- expired-token assertion failed

Evidence:

- `evidence/CP-1.3_T04 FAILED (as expected).png`
- `postman/ExpiredToken.postman_collection.json`

**Result**

⚠️ Security finding — the captured run indicates token expiration was not enforced.

## Tooling

- Postman
- JWT Bearer token scenarios
- Postman assertions
- environment variables including `base_url` and token values

## Finding

The captured checkpoint results indicate **inconsistent JWT authentication enforcement** on the list endpoint.

The strongest supported interpretation is:

- missing token accepted in the captured run
- invalid token accepted in the captured run
- valid token accepted as expected
- expired token accepted in the captured run

This is an authentication-control problem rather than an object-level authorization finding.

## Risk

If reproduced in production, potential impact includes:

- unauthenticated access to user-list data
- ineffective invalid-token rejection
- session lifetime not enforced
- increased exposure window for leaked expired tokens

Potential severity depends on data sensitivity and production architecture.

## Recommendation

1. Centralize JWT verification for protected endpoints.
2. Reject missing authentication.
3. Reject malformed or invalid tokens.
4. Verify JWT signature.
5. Enforce `exp`.
6. Validate `iss` and `aud` where required by the trust model.
7. Add automated negative authentication tests to CI/CD.
8. Ensure security tests fail the pipeline when protected endpoints accept invalid authentication states.

## Evidence

- `evidence/CP-1.3_T01_no_token_list_exposed_200.png`
- `evidence/CP-1.3_T02_invalid_token_list_200.png`
- `evidence/CP-1.3_T03_valid_token_get_users_200.png`
- `evidence/CP-1.3_T04 FAILED (as expected).png`
- `postman/CP-1.3_postman_collection_v1.json`
- `postman/ExpiredToken.postman_collection.json`
- `postman/CP-1.3_postman_environment_idor-auth-env_v1.json`

## Limitation

This README documents the behavior represented by the committed evidence and Postman assets.
A production assessment should repeat the matrix against the current deployed implementation
and capture normalized response bodies, status codes, and token metadata.

## Status

**CP-1.3 — COMPLETED WITH SECURITY FINDINGS**

- T01 — No Token ⚠️
- T02 — Invalid Token ⚠️
- T03 — Valid Token ✅
- T04 — Expired Token ⚠️
