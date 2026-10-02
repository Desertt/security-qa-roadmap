[🇬🇧 English](README.md) | [🇹🇷 Türkçe](README.tr.md)

# CP-1.1 — Initial IDOR / Object Access Probe

## Goal

Establish an initial baseline for user-resource access behavior and prepare the test surface
for later IDOR / Broken Object Level Authorization (BOLA) validation.

This checkpoint is an **initial access-control probe**, not a definitive authenticated BOLA test.

## Target

Observed repository artifacts reference:

- `GET /users`
- `GET /users/{user_id}`

The committed Postman collection includes an unauthenticated object-access check against
`/users/user_103`.

## Threat / Negative Test

The initial question is:

> Can user-related resources be retrieved without a security context that proves the requester is authorized?

A confirmed BOLA result would require an authenticated principal, an object owned by another principal,
and evidence that the first principal can access that object despite the authorization boundary.

That complete identity/ownership context is not present in this checkpoint.

## Evidence

Committed evidence files:

- `evidence/idor_detected_users_list_200.png`
- `evidence/idor_protected_non_existing_user_404.png`

The repository also contains:

- `postman/idor.postman_collection`
- `postman/idor-env.postman_environment.json`

The Postman collection contains a negative assertion stating that another user's resource
should not return `200 OK`.

## Finding

The committed artifacts show user-resource access behavior that warranted further access-control testing.

However, the checkpoint does **not contain enough authenticated identity and ownership context**
to prove a cross-user BOLA vulnerability.

### Classification

**Initial access-control concern / BOLA validation pending**

The result should not be represented as a confirmed production BOLA finding.

## Potential Risk

If user objects are accessible without authentication or without object-level authorization in a production system,
possible impact could include:

- unauthorized user-data access
- object enumeration
- privacy exposure
- cross-user or cross-tenant data access

Potential severity depends on the data exposed and the production authorization model.

## Recommendation

For a stronger follow-up:

1. Require authentication for protected user resources.
2. Establish at least two test identities: User A and User B.
3. Define ownership or authorization rules for each object.
4. Verify that User A can access User A's object.
5. Verify that User A cannot access User B's object.
6. Repeat the same boundary check in the opposite direction.
7. Capture expected, actual, and evidence for every scenario.

## Limitation / Next Validation

The current evidence and Postman collection reference different specific access checks.
The next run should normalize scenario IDs, endpoint targets, identities, and expected outcomes.

## Status

**CP-1.1 — INITIAL PROBE COMPLETED**

**Follow-up:** authenticated BOLA / object-ownership validation required.
