[🇬🇧 English](README.md) | [🇹🇷 Türkçe](README.tr.md)

# CP-1.2 — Authentication / JWT Enforcement Baseline

## Goal

Evaluate whether sensitive user endpoints enforce authentication and reject invalid authentication input.

## Scope

Primary repository artifacts reference:

- `GET /users/`
- `GET /users/{id}`

Tooling:

- Postman
- JWT / Bearer token scenarios
- Postman test assertions

## Test Scenarios

### AUTH-01 — No Token — `GET /users`

**Expected**

- `401 Unauthorized` or `403 Forbidden`

**Committed evidence**

- `evidence/auth_01_no_token_200_failed.png`

The evidence filename records a `200` response for the no-token run.

**Finding**

⚠️ Authentication enforcement concern: the captured run indicates the endpoint returned data without a token.

---

### AUTH-02 — Invalid Token — `GET /users`

**Expected**

- `401 Unauthorized`

**Committed evidence**

- `evidence/auth_02_invalid_token_200_failed.png`

The evidence filename records a `200` response for the invalid-token run.

**Finding**

⚠️ Invalid-token enforcement concern: the captured run indicates the invalid credential did not cause rejection.

---

### AUTH-03 — Valid Token / Other User — `GET /users/{id}`

The Postman collection defines a negative assertion that access to another user's resource
must not return `200 OK`.

Committed evidence:

- `evidence/auth_03_self_other_200.png`

The previous README described the other-user result as `404 Not Found`, while the committed
evidence filename contains `200`.

Because these repository sources are inconsistent, this scenario should **not be presented as a confirmed pass or failure**
until it is rerun with normalized identities and evidence.

**Status:** Revalidation required.

## Finding Summary

The strongest supported conclusion from the committed evidence is:

- no-token access was captured with `200`
- invalid-token access was captured with `200`
- the cross-user authorization scenario is inconsistent across repository artifacts

This checkpoint therefore supports an **authentication enforcement finding**,
while the object-level authorization result remains unresolved.

## Risk

If a sensitive endpoint accepts requests without valid authentication in production:

- unauthorized data access may occur
- invalid credentials may be ignored
- authentication boundaries may be bypassed
- downstream authorization controls may not receive a trustworthy principal

Potential severity can be high depending on the exposed data and production architecture.

## Recommendation

1. Enforce authentication on protected endpoints.
2. Reject missing credentials with an appropriate unauthenticated response.
3. Verify JWT signature and token structure.
4. Validate expiration (`exp`) and, where applicable, issuer (`iss`) and audience (`aud`).
5. Establish a trusted authenticated principal before object-level authorization is evaluated.
6. Rerun cross-user tests with at least two known identities and explicit ownership rules.

## Evidence

- `evidence/auth_01_no_token_200_failed.png`
- `evidence/auth_02_invalid_token_200_failed.png`
- `evidence/auth_03_self_other_200.png`
- `postman/auth.postman_collection.json`
- `postman/idor-auth-env.postman_environment.json`

No secret values should be committed to the repository.

## Status

**CP-1.2 — AUTHENTICATION BASELINE COMPLETED**

- AUTH-01 — Finding ⚠️
- AUTH-02 — Finding ⚠️
- AUTH-03 — Revalidation required 🟡
