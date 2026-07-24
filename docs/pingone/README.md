# PingOne

Learn PingOne end to end — directory, SSO (OIDC / SAML), Worker apps, and SCIM provisioning — with copy-paste examples grounded in a real Spring Boot integration project.

**Copyright © 2026 YankiDev ([yankidev.com](https://yankidev.com))**

| Field | Value |
|-------|-------|
| Status | stable |
| Audience | engineers, architects, IAM / IGA practitioners |
| Level | intro → advanced |
| Last reviewed | 2026-07-24 |
| Source project | Companion repo `pingone` (Spring Boot OIDC template + SCIM/SailPoint docs) |

## Start here

| Goal | Go to |
|------|--------|
| New to PingOne | [Overview](01-overview.md) → [Glossary](02-glossary.md) |
| Understand the big picture | [Architecture](03-architecture.md) |
| Implement login (OIDC) | [OIDC](oidc/README.md) |
| Implement SAML SSO | [SAML](saml/README.md) |
| Provision users (SCIM) | [SCIM](scim/README.md) |
| Follow a guided path | [Learning path](learning-path.md) |
| Compare protocols | [Protocols comparison](protocols-comparison.md) |

## What you will learn

1. **PingOne as IDaaS** — environment, population, directory, access engine  
2. **OIDC** — Authorization Code, PKCE (SPA/Native), Client Credentials (Worker), discovery, JWKS, logout  
3. **SAML 2.0** — SP/IdP roles, metadata, ACS, Spring Security SAML config  
4. **SCIM 2.0** — RFC 7642/7643/7644, PingOne SCIM gateway, JML (Joiner/Mover/Leaver)  
5. **Enterprise integration** — SailPoint (IGA) → Worker token → PingOne SCIM  

## Folder map

```text
docs/pingone/
├── README.md                 ← you are here
├── 01-overview.md
├── 02-glossary.md
├── 03-architecture.md
├── learning-path.md
├── protocols-comparison.md
├── reference-source-project.md
├── oidc/                     ← OpenID Connect + OAuth grants
├── saml/                     ← SAML 2.0 SSO
└── scim/                     ← SCIM provisioning + SailPoint
```

## Hands-on companion

Examples in this section are adapted from the **pingone** Spring Boot template (OIDC test client + starter). See [Reference: source project](reference-source-project.md) for module layout, how to run mock/real mode, and where each example originated.

> Replace `{env-id}` / `{environment-id}` with your real PingOne environment UUID. Do not leave braces in live config — Spring treats `{...}` as URI template variables.
