# PingOne overview

| Field | Value |
|-------|-------|
| Status | stable |
| Audience | engineers, architects |
| Level | intro |
| Last reviewed | 2026-07-24 |
| Owner | YankiDev |

**Copyright © 2026 YankiDev ([yankidev.com](https://yankidev.com))**

## Summary

**PingOne** is a cloud-native, multi-tenant Identity-as-a-Service (IDaaS) platform. In enterprise architectures it usually plays two roles at once:

1. **Core directory** — stores workforce / customer identities, credentials, and group memberships (partitioned by **Environment** and **Population**)
2. **Access engine** — SSO, MFA, federation (**OIDC**, **SAML**), and token issuance for applications and APIs

Provisioning systems (for example **SailPoint**) typically manage identities *into* PingOne over **SCIM**, while applications authenticate users *through* PingOne over **OIDC** or **SAML**.

## Core building blocks

| Concept | What it means |
|---------|----------------|
| **Environment** | Tenant partition. UUID appears in issuer and API URLs |
| **Population** | Directory partition for users (required on many SCIM creates) |
| **Application** | OIDC Web App, SPA, Native, Worker, Device, or SAML app registered in PingOne Admin |
| **Worker** | Non-interactive API client (client credentials) used by IGA / automation |
| **Issuer** | OIDC base URL: `https://auth.pingone.com/{env-id}/as` |
| **SCIM base** | `https://api.pingone.com/v1/environments/{env-id}/scim/v2` |

## Application types (PingOne Admin)

| Type | Config value (Spring template) | Typical grant / protocol | Interactive user? |
|------|--------------------------------|--------------------------|-------------------|
| OIDC Web App | `oidc-web-app` | Authorization Code (+ secret) | Yes |
| OIDC SPA | `oidc-spa` | Authorization Code + **PKCE** (public) | Yes |
| OIDC Native | `oidc-native` | Authorization Code + **PKCE** | Yes |
| Worker | `worker` | **Client Credentials** | No |
| Device | `device` | Device Authorization Grant | Yes (on second device) |
| SAML | `saml` | SAML 2.0 assertions | Yes |

## Two planes you must not confuse

```text
┌─────────────────────┐         ┌─────────────────────┐
│  Provisioning plane │  SCIM   │     PingOne         │
│  (SailPoint / IGA)  │ ──────► │  Directory          │
│  uses Worker token  │         │  Populations/Users  │
└─────────────────────┘         └──────────┬──────────┘
                                           │ OIDC / SAML
                                           ▼
                                ┌─────────────────────┐
                                │  Application plane  │
                                │  (your apps / SSO)  │
                                └─────────────────────┘
```

- **SCIM** answers: “Create / update / disable this identity.”
- **OIDC / SAML** answers: “Is this user allowed to sign in to this app right now?”

## What to read next

1. [Glossary](02-glossary.md)  
2. [Architecture](03-architecture.md)  
3. Choose a protocol track: [OIDC](oidc/README.md) · [SAML](saml/README.md) · [SCIM](scim/README.md)
