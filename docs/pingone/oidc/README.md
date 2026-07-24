# OpenID Connect (OIDC) with PingOne

OIDC is the primary protocol for modern app login against PingOne. This track goes from concepts to copy-paste Spring Boot configuration adapted from the companion `pingone` project.

**Copyright © 2026 YankiDev ([yankidev.com](https://yankidev.com))**

## Contents

| Document | Level | Focus |
|----------|-------|--------|
| [OIDC basics](basics.md) | intro | Actors, tokens, discovery, Authorization Code flow |
| [Web App (Authorization Code)](authorization-code-web-app.md) | intro–intermediate | Confidential client + Admin checklist + YAML |
| [SPA & Native (PKCE)](spa-native-pkce.md) | intermediate | Public clients, no secret |
| [Worker (Client Credentials)](worker-client-credentials.md) | intermediate | Machine tokens for SCIM/API |
| [Discovery, JWKS, logout](discovery-jwks-logout.md) | intermediate | Metadata, validation, RP-initiated logout |
| [Spring Boot examples](spring-boot-examples.md) | intermediate–advanced | Security config, Java snippets, endpoints |

## Mental model

```text
Browser / App  --authorize-->  PingOne (OP)
               <--code-------
App            --token------>  PingOne /as/token
               <--id+access--
App validates ID Token (JWKS), creates session
```

## Quick Admin defaults (local OIDC Web App)

| Setting | Value |
|---------|--------|
| Application type | OIDC Web App |
| Grant | Authorization Code |
| Redirect URI | `http://localhost:8080/login/oauth2/code/pingone` |
| Post-logout redirect | `http://localhost:8080/` |
| Scopes | `openid`, `profile`, `email` |

## Related

- [Protocols comparison](../protocols-comparison.md)
- [SCIM](../scim/README.md) (uses Worker tokens)
