[🇬🇧 English](README.md) | [🇹🇷 Türkçe](README.tr.md)

# CP-1.4 — Object ID Manipulation / BOLA Validation Preparation

## Goal

Evaluate object-access behavior on `GET /users/{userId}` and determine whether the current
test evidence is sufficient to prove Broken Object Level Authorization (BOLA).

BOLA requires an authorization boundary between an authenticated requester and a target object.
The current checkpoint does not establish a controlled two-principal ownership matrix,
so it should be treated as **BOLA validation preparation**, not definitive BOLA proof.

## Endpoint Under Test

- **Method:** GET
- **Path:** `/users/{userId}`

## Test Scenarios

### T01 — Baseline Object Access

Request:

`GET /users/user_100`

The committed Postman collection configures this request with `noauth`.

Evidence:

- `evidence/CP-1.4-T01_01_get-user-100_200.png`

Captured result:

- `200 OK`

**Interpretation:** baseline object retrieval confirmed.

---

### T02 — Object ID Manipulation

Request:

`GET /users/user_101`

The committed Postman collection sends an invalid bearer token for this request.

Evidence:

- `evidence/CP-1.4-T01_02_get-user-101_200.png`

Captured result:

- `200 OK`

**Interpretation:** changing the object ID still returned an existing user object in the captured run,
despite the request containing an invalid token.

This is an **authentication / authorization enforcement concern**.

It does **not yet prove BOLA**, because the run does not establish a valid authenticated User A
attempting to access an object owned by User B.

---

### T03 — Nonexistent Object

Request:

`GET /users/user_999999`

The committed Postman collection uses a bearer token variable for this request.

Evidence:

- `evidence/CP-1.4-T01_03_get-user-999999_404.png`

Captured result:

- `404 Not Found`

**Interpretation:** nonexistent-object handling is confirmed.

## Tooling

- Postman
- manual API execution
- Bearer token scenarios
- environment variables

## Finding

### Object Access Is Observable, but Authenticated BOLA Is Not Yet Proven

The current artifacts demonstrate:

- existing user object returned with no authentication context in the baseline run
- changed object ID returned `200` in a request carrying an invalid token
- nonexistent object returned `404`

These observations justify further access-control testing.

However, a definitive BOLA finding requires:

- a valid authenticated principal
- a second principal
- known object ownership
- a documented authorization boundary
- proof that the first principal can access the second principal's object

That evidence is not present in the current checkpoint.

### Classification

**Access-control / authentication concern — authenticated BOLA validation pending**

## Potential Production Risk

If authenticated users can modify object identifiers and access objects outside their authorization boundary,
potential impact can include:

- cross-user data access
- cross-tenant exposure
- object enumeration
- privacy violations
- unauthorized modification or disclosure, depending on endpoint capability

**Potential Severity: High**

Severity is contextual and should be assigned only after the production authorization model
and affected data are understood.

## Recommendation

1. Enforce authentication before protected object access.
2. Establish a trusted principal from validated credentials.
3. Apply object-level authorization based on ownership, role, tenant, or policy.
4. Deny access by default when authorization cannot be established.
5. Use an appropriate response:
   - `401 Unauthorized` for missing/invalid authentication
   - `403 Forbidden` for authenticated but unauthorized access
   - optionally `404 Not Found` if object-existence concealment is part of the design
6. Add automated cross-user authorization tests.

## Next Validation Matrix

A future BOLA validation should include at least:

| Scenario | Expected |
|---|---|
| User A → User A object | Allowed |
| User A → User B object | Denied |
| User B → User A object | Denied |
| Missing token → protected object | Denied |
| Invalid token → protected object | Denied |
| Privileged role → target object | Policy-dependent |

## Evidence

Legacy evidence filenames are retained as committed:

- `evidence/CP-1.4-T01_01_get-user-100_200.png`
- `evidence/CP-1.4-T01_02_get-user-101_200.png`
- `evidence/CP-1.4-T01_03_get-user-999999_404.png`

Supporting Postman assets:

- `postman/cp-1.4-idor-bola.postman_collection`
- `postman/idor-auth-env.postman_environment.json`

## Limitation

The existing artifact names use `T01_01`, `T01_02`, and `T01_03`.
The documentation maps these legacy evidence files to normalized scenarios `T01`, `T02`, and `T03`
without renaming the committed evidence.

## Status

**CP-1.4 — CURRENT RUN COMPLETED**

- T01 — Baseline Object Access ✅
- T02 — Object ID Manipulation ⚠️ Access-control concern
- T03 — Nonexistent Object Handling ✅

**Follow-up:** authenticated two-principal BOLA validation required.
