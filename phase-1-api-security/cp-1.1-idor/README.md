# CP-1.1 – IDOR (Insecure Direct Object Reference)

## Target
GET /users  
GET /users/{user_id}

## Description
The API allows access to user resources without proper object-level authorization.
An unauthorized request to `/users` returns HTTP 200, indicating a Broken Object Level Authorization vulnerability.

## Evidence
- Unauthorized request returned **200 OK** (IDOR detected)
- Non-existing user correctly returned **404 Not Found** (control case)

## OWASP Mapping
- OWASP API Security Top 10: **API1 – Broken Object Level Authorization**

## Severity
High

## Tooling
- Postman
- Manual API testing
