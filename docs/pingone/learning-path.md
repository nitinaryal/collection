# Learning path — PingOne (basic → advanced)

| Field | Value |
|-------|-------|
| Status | stable |
| Audience | all learners |
| Level | intro → advanced |
| Last reviewed | 2026-07-24 |
| Owner | YankiDev |

**Copyright © 2026 YankiDev ([yankidev.com](https://yankidev.com))**

Follow in order, or jump to the track you need.

## Basic (orientation)

| Step | Document | Done when |
|------|----------|-----------|
| 1 | [Overview](01-overview.md) | You can explain directory vs access engine |
| 2 | [Glossary](02-glossary.md) | You recognize Environment, Population, Worker, Issuer |
| 3 | [Architecture](03-architecture.md) | You can sketch AD → SailPoint → PingOne → Apps |
| 4 | [Protocols comparison](protocols-comparison.md) | You know when to pick OIDC vs SAML vs SCIM |

## Intermediate — OIDC SSO

| Step | Document | Done when |
|------|----------|-----------|
| 5 | [OIDC basics](oidc/basics.md) | You can narrate Authorization Code steps |
| 6 | [Web App copy-paste](oidc/authorization-code-web-app.md) | Admin checklist + YAML configured |
| 7 | [Discovery / JWKS / logout](oidc/discovery-jwks-logout.md) | You validate issuer + post-logout URI |
| 8 | [Spring Boot examples](oidc/spring-boot-examples.md) | You run mock or real companion client |

## Intermediate — public clients & Worker

| Step | Document | Done when |
|------|----------|-----------|
| 9 | [SPA / Native PKCE](oidc/spa-native-pkce.md) | You know why secrets are forbidden |
| 10 | [Worker credentials](oidc/worker-client-credentials.md) | You obtain a machine token with curl |

## Intermediate — SAML

| Step | Document | Done when |
|------|----------|-----------|
| 11 | [SAML basics](saml/basics.md) | You know Entity ID, ACS, metadata |
| 12 | [SAML Spring examples](saml/spring-boot-examples.md) | YAML + Admin checklist ready |

## Advanced — SCIM / IGA

| Step | Document | Done when |
|------|----------|-----------|
| 13 | [SCIM basics](scim/basics.md) | You can explain RFC 7642/7643/7644 roles |
| 14 | [Protocol & schemas](scim/protocol-and-schemas.md) | You use Resource / Schema / Resource Type correctly |
| 15 | [PingOne SCIM API](scim/pingone-scim-api.md) | You know base URL + population requirement |
| 16 | [JML use cases](scim/jml-use-cases.md) | You map Joiner/Mover/Leaver to HTTP verbs |
| 17 | [SailPoint prep](scim/sailpoint-integration.md) | You can run a Phase 1–3 checklist |
| 18 | [SCIM copy-paste](scim/copy-paste-examples.md) | Token + create + PATCH deactivate works in sandbox |

## Capstone exercises

1. **SSO lab** — Configure OIDC Web App; login; inspect `/me` claims; logout with registered post-logout URI.  
2. **Worker lab** — Client credentials token; call `GET .../scim/v2/Schemas`.  
3. **JML lab** — Create user with population; PATCH department; set `active=false`.  
4. **Decision lab** — For a legacy portal vs SPA vs IGA feed, pick SAML / OIDC / SCIM and justify.

## Related

- [Reference: source project](reference-source-project.md)
