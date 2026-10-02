[🇬🇧 English](README.md) | [🇹🇷 Türkçe](README.tr.md)

# Phase 1 — API & Application Security

Phase 1 contains hands-on API security checkpoints built around negative testing,
authentication behavior, object access, evidence capture, and risk-oriented analysis.

## Checkpoints

| Checkpoint | Focus | Current Interpretation |
|---|---|---|
| [CP-1.1](cp-1.1-idor/) | Initial IDOR / object-access probe | Early access-control probe; definitive BOLA validation requires authenticated ownership context |
| [CP-1.2](cp-1.2-auth-jwt/) | Authentication / JWT baseline | No-token and invalid-token evidence indicate authentication enforcement findings; cross-user result requires revalidation |
| [CP-1.3](cp-1.3-list-endpoint-leakage/) | List endpoint authentication matrix | Captured runs show inconsistent JWT enforcement, including expired-token acceptance |
| [CP-1.4](cp-1.4-idor-bola/) | Object ID manipulation / BOLA preparation | Object-access behavior captured, but authenticated cross-user BOLA is not yet proven |

## Evidence Standard

Each checkpoint should document:

1. Goal
2. Endpoint under test
3. Test scenario
4. Expected behavior
5. Actual behavior
6. Evidence
7. Finding
8. Risk
9. Recommendation
10. Limitation / next validation

A security label should not be treated as confirmed unless the evidence supports the required security context.

For BOLA specifically, a strong validation normally requires at least two authenticated identities
and a clear object-ownership or authorization boundary.

## Tooling

Current repository evidence includes:

- Postman collections
- Postman environments
- manual API execution
- test assertions
- screenshots
- JWT-oriented negative scenarios

## Safety

All tests are intended for authorized or lab environments only.
