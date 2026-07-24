# PingOne architecture (enterprise view)

| Field | Value |
|-------|-------|
| Status | stable |
| Audience | architects, IAM / IGA engineers |
| Level | intermediate |
| Last reviewed | 2026-07-24 |
| Owner | YankiDev |

**Copyright © 2026 YankiDev ([yankidev.com](https://yankidev.com))**

## Summary

A common enterprise pattern: **Active Directory** (or HR) as source of truth → **SailPoint** as IGA / SCIM client → **PingOne** as cloud directory + SSO. Applications then authenticate users with **OIDC** or **SAML**.

This diagram is adapted from the companion project architecture notes (`LDAP_Sailpoint_SCIM_Protocol_PingOne_Architecutre.txt`).

## End-to-end diagram

```text
+-----------------------------+
|      Active Directory       |
|      (Source of Truth)      |
+-----------------------------+
               |
               | LDAP/LDAPS
               v
+-----------------------------+
|          SailPoint          |
|  • Aggregation / mapping    |
|  • Policy / SoD             |
|  • SCIM client engine       |
+-----------------------------+
        |                 |
 1. Token (client_creds)  |  2. SCIM REST + Bearer
        v                 v
+--------------------+  +--------------------+
| PingOne Worker App |  | PingOne SCIM API   |
| /as/token          |  | .../scim/v2        |
+--------------------+  +--------------------+
                                  |
                                  v
                    +-----------------------------+
                    |     PingOne Environment     |
                    |  Populations / Users/Groups |
                    +-----------------------------+
                                  |
                     OIDC / SAML SSO
                                  v
                    +-----------------------------+
                    |     Business applications   |
                    +-----------------------------+
```

## Why Worker exists

SailPoint is an **administrative machine**, not an end user. In PingOne, **Worker** is the application type that:

- Authenticates with **client credentials** (no browser)
- Can hold **platform admin roles** (e.g. Identity Data Admin, Identity Router, User Admin)
- Obtains Bearer tokens used against the **SCIM** and management APIs

OIDC Web App / SPA / SAML apps are for **user SSO** and cannot replace Worker for headless directory management.

## URL cheat sheet

| Purpose | URL pattern |
|---------|-------------|
| OIDC issuer | `https://auth.pingone.com/{env-id}/as` |
| Token endpoint | `https://auth.pingone.com/{env-id}/as/token` |
| OIDC discovery | `https://auth.pingone.com/{env-id}/as/.well-known/openid-configuration` |
| SAML IdP metadata | `https://auth.pingone.com/{env-id}/saml20/metadata` |
| SCIM base | `https://api.pingone.com/v1/environments/{env-id}/scim/v2` |
| Native PingOne API | `https://api.pingone.com/v1/environments/{env-id}/...` |

## Native API vs SCIM gateway

| | Native (`.../v1/...`) | SCIM (`.../scim/v2/...`) |
|--|----------------------|---------------------------|
| Style | PingOne proprietary (HAL-style `_links`, `_embedded`) | IETF SCIM JSON + URNs |
| Consumers | Admin UI, custom tools | SailPoint, Entra, Okta, standard SCIM clients |
| Schema | Platform models | `urn:ietf:params:scim:schemas:core:2.0:User`, etc. |

Prefer **SCIM** for IGA interoperability; use **native API** when you need PingOne-specific capabilities not exposed via SCIM.

## Related

- [SCIM + SailPoint](scim/sailpoint-integration.md)
- [Worker / client credentials](oidc/worker-client-credentials.md)
- [Source project](reference-source-project.md)
