# PingOne glossary

| Field | Value |
|-------|-------|
| Status | stable |
| Audience | engineers, leads, architects |
| Level | intro |
| Last reviewed | 2026-07-24 |
| Owner | YankiDev |

**Copyright © 2026 YankiDev ([yankidev.com](https://yankidev.com))**

| Term | Meaning |
|------|---------|
| **ACS** | Assertion Consumer Service — SP endpoint that receives SAML assertions |
| **Authorization Code** | OAuth/OIDC grant for browser apps: code exchanged at token endpoint |
| **Client Credentials** | Machine-to-machine OAuth grant (Worker apps; no user login) |
| **DaVinci** | Ping Identity orchestration product for custom auth journeys |
| **Discovery** | OIDC metadata at `{issuer}/.well-known/openid-configuration` |
| **End session / signoff** | RP-initiated logout against PingOne |
| **Entity ID** | Unique SAML SP identifier registered with the IdP |
| **Environment** | PingOne tenant partition (`{env-id}` in URLs) |
| **IdP / OP** | Identity Provider / OpenID Provider — PingOne in SSO scenarios |
| **IGA** | Identity Governance and Administration (e.g. SailPoint) |
| **Issuer URI** | `https://auth.pingone.com/{env-id}/as` |
| **JML** | Joiner / Mover / Leaver identity lifecycle |
| **JWKS** | JSON Web Key Set used to validate ID/access token signatures |
| **OIDC** | OpenID Connect — identity layer on OAuth 2.0 |
| **PKCE** | Proof Key for Code Exchange — required for public clients (SPA/Native) |
| **Population** | PingOne directory partition; often required on SCIM user create |
| **Registration ID** | Spring OAuth2 client key; must match redirect path suffix |
| **RP** | Relying Party — your application trusting PingOne |
| **SAML** | Security Assertion Markup Language 2.0 (XML assertions) |
| **SCIM** | System for Cross-domain Identity Management (RFC 7642/7643/7644) |
| **SP** | Service Provider — your app in SAML terms |
| **SSO** | Single Sign-On |
| **Worker** | PingOne app type for headless admin/API access with platform roles |

## Related

- [Overview](01-overview.md)
- [Protocols comparison](protocols-comparison.md)
