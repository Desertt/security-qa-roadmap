[🇬🇧 English](README.md) | [🇹🇷 Türkçe](README.tr.md)

# Security QA Roadmap

Hands-on engineering portfolio focused on the transition from **Senior QA / Test Automation**
toward **Security-Aware Quality Engineering, Security QA, Application Security, and DevSecOps**.

The repository applies a practical, evidence-driven approach to security testing by combining
QA automation experience with API security, negative testing, secure software delivery,
traceability, and repeatable technical evidence.

## Engineering Approach

Each checkpoint follows the same evidence chain:

**Goal → Target Endpoint → Threat / Negative Test → Execution → Evidence → Finding → Risk → Recommendation → Limitation / Next Validation**

The objective is not only to identify suspicious behavior, but to show:

- what was tested
- what evidence supports the result
- what can and cannot be concluded from the current run
- what the potential security impact is
- what should be validated next

## Repository Structure

```text
phase-1-api-security/
├── README.md
├── README.tr.md
├── cp-1.1-idor/
│   ├── README.md
│   └── README.tr.md
├── cp-1.2-auth-jwt/
│   ├── README.md
│   └── README.tr.md
├── cp-1.3-list-endpoint-leakage/
│   ├── README.md
│   └── README.tr.md
└── cp-1.4-idor-bola/
    ├── README.md
    └── README.tr.md
```

## Current Focus

### Phase 1 — API & Application Security Fundamentals

Current checkpoints cover:

- initial object-access and IDOR/BOLA-oriented probes
- authentication and JWT enforcement
- list-endpoint exposure and token-state validation
- object identifier manipulation
- negative security testing
- evidence-based findings and limitations

Primary tools and practices currently represented in the repository include:

- Postman
- REST API testing
- JWT-oriented negative tests
- response/status validation
- evidence capture
- risk-oriented analysis
- OWASP API Security concepts

## Roadmap

### Phase 1 — API & Application Security
API security fundamentals and evidence-driven negative testing.

### Phase 2 — Security Automation & DevSecOps
Repeatable security checks integrated into automation and CI/CD workflows.

### Phase 3 — Offensive Security Awareness
Safe lab-based exercises for understanding attacker techniques and improving defensive testing.

### Phase 4 — Platform & Cloud Security Foundations
Security concepts related to identity, cloud environments, platforms, and infrastructure.

### Phase 5 — Portfolio Hardening
Improve reproducibility, evidence quality, technical documentation, and project presentation.

### Phase 6 — Job-Ready Security QA Package
Consolidate practical work into a recruiter-ready Security QA / DevSecOps engineering portfolio.

## Documentation Standard

`README.md` is the English source of truth.

Where available, `README.tr.md` is the Turkish companion document. Technical conclusions,
evidence references, limitations, and checkpoint status should remain synchronized between languages.

## Portfolio Goal

This repository demonstrates how a Quality Engineering background can be extended into
security-aware engineering through:

- repeatable security testing
- API security validation
- evidence and traceability
- risk-based thinking
- explicit test limitations
- security automation
- DevSecOps practices

The focus is on **hands-on engineering evidence**, not only theoretical security knowledge.

## Safety

All security exercises in this repository are intended for systems that are owned,
authorized for testing, or operated as safe lab environments.

No checkpoint should be interpreted as permission to test third-party systems without authorization.
