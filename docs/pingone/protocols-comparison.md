# Protocols comparison (OIDC · SAML · SCIM · OAuth)

| Field | Value |
|-------|-------|
| Status | stable |
| Audience | architects, leads, engineers |
| Level | intro–intermediate |
| Last reviewed | 2026-07-24 |
| Owner | YankiDev |

**Copyright © 2026 YankiDev ([yankidev.com](https://yankidev.com))**

## Choose the right protocol

| Need | Protocol | PingOne piece |
|------|----------|---------------|
| User signs into a modern web/API app | **OIDC** | OIDC Web App / SPA / Native |
| User signs into a legacy enterprise app expecting XML | **SAML** | SAML application |
| Create/update/disable identities from IGA/HR | **SCIM** | SCIM gateway + **Worker** token |
| Backend calls PingOne APIs with no user | **OAuth Client Credentials** | **Worker** |
| TV / CLI device login | **Device Authorization** (OAuth) | Device app (advanced) |

## Side-by-side

| | OIDC | SAML | SCIM |
|--|------|------|------|
| **Plane** | Authentication / SSO | Authentication / SSO | Provisioning / directory |
| **Primary artifact** | ID Token (JWT) | Assertion (XML) | User/Group JSON resources |
| **Typical client** | App (RP) | App (SP) | IGA (SailPoint) |
| **User present?** | Yes | Yes | No (admin machine) |
| **Auth to PingOne API** | N/A (user SSO) | N/A | Worker Bearer token |

## OAuth grants you will meet in PingOne apps

| Grant | App type | Notes |
|-------|----------|-------|
| `authorization_code` | Web / SPA / Native | Add PKCE for public clients |
| `client_credentials` | Worker | SCIM & platform APIs |
| `refresh_token` | Interactive (if issued) | Silent renew |
| Device code | Device | Polling + second-device login |

## Anti-patterns

- Using SCIM for interactive login  
- Using OIDC Web App secrets inside a SPA  
- Expecting SAML apps to hold Identity Data Admin roles for provisioning  
- Leaving `{env-id}` placeholders in production issuer URLs  

## Related tracks

- [OIDC](oidc/README.md) · [SAML](saml/README.md) · [SCIM](scim/README.md)
