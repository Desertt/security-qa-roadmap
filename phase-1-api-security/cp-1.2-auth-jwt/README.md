# CP-1.2 — Broken Authentication (Auth/JWT)

## Scope
- Target: `GET /users/` (List Users)
- Tools: Postman (+ scripts)

## Summary
Two tests show the API does not enforce authentication on a sensitive endpoint.

## Findings

### AUTH-01 — No Token — GET /users
**Expected:** 401/403  
**Actual:** 200 OK  
**Result:** VULNERABLE (Authentication not enforced)

### AUTH-02 — Invalid Token — GET /users
**Expected:** 401  
**Actual:** 200 OK  
**Result:** VULNERABLE (Token validation bypass / missing)

### AUTH-03 — Valid Token but Other User — GET /users/{id}
- Own user: 200 OK
- Other user: 404 Not Found
**Result:** Access to other user's resource is prevented (resource hiding).  
> Note: This does not mitigate the list endpoint exposure.

## Impact
Unauthorized users can access user list data without any valid token.

## Recommendation (High level)
- Enforce auth on `GET /users/` via middleware/dependency (JWT verification)
- Reject invalid/expired tokens (signature + exp + audience/issuer checks)
- Apply consistent authorization policy across endpoints