# SAML 2.0 with PingOne

SAML remains common for enterprise apps that expect XML assertions instead of OIDC tokens. PingOne acts as the **Identity Provider (IdP)**; your application is the **Service Provider (SP)**.

**Copyright © 2026 YankiDev ([yankidev.com](https://yankidev.com))**

## Contents

| Document | Level | Focus |
|----------|-------|--------|
| [SAML basics](basics.md) | intro | Actors, assertion flow, metadata |
| [Spring Boot examples](spring-boot-examples.md) | intermediate | Copy-paste YAML, Admin checklist, UI sample |

## Quick comparison vs OIDC

| | SAML | OIDC |
|--|------|------|
| Message format | XML assertions | JWT (ID Token) + JSON |
| Typical apps | Legacy / enterprise portals | Modern web/API / mobile |
| Trust bootstrap | IdP + SP metadata | Discovery + JWKS |
| Spring stack | `spring-security-saml2-service-provider` | `spring-boot-starter-oauth2-client` |

## Related

- [Protocols comparison](../protocols-comparison.md)
- [OIDC track](../oidc/README.md)
