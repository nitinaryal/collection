# OIDC SPA & Native — PKCE public clients (copy-paste)

| Field | Value |
|-------|-------|
| Status | stable |
| Audience | engineers |
| Level | intermediate |
| Last reviewed | 2026-07-24 |
| Owner | YankiDev |

**Copyright © 2026 YankiDev ([yankidev.com](https://yankidev.com))**

Adapted from the companion project’s public-client adoption generators (`oidc-spa`, `oidc-native`).

## Why PKCE?

SPA and native apps **cannot keep a client secret**. They use:

- `client-authentication-method: none`
- Authorization Code + **PKCE** (`code_verifier` / `code_challenge`)

Spring Security OAuth2 Client enables PKCE automatically for public clients.

## PingOne Admin checklist (SPA)

- [ ] Application type: **OIDC Single Page App** (or equivalent public client)
- [ ] Grant: Authorization Code + PKCE
- [ ] **No** client secret
- [ ] Redirect URI = SPA callback route (e.g. `https://app.example.com/callback`)
- [ ] Post-logout redirect registered
- [ ] Scopes: `openid`, `profile`, `email`

## PingOne Admin checklist (Native)

- [ ] Application type: **Native**
- [ ] Grant: Authorization Code + PKCE
- [ ] Redirect: loopback (`http://127.0.0.1:...`) or custom URI scheme
- [ ] No client secret

## Copy-paste: SPA `application.yml`

```yaml
server:
  port: 8080

spring:
  security:
    oauth2:
      client:
        registration:
          pingone:
            client-id: ${PINGONE_CLIENT_ID}
            client-authentication-method: none
            authorization-grant-type: authorization_code
            redirect-uri: https://app.example.com/callback
            scope:
              - openid
              - profile
              - email
            provider: pingone
        provider:
          pingone:
            issuer-uri: https://auth.pingone.com/YOUR-ENV-UUID/as
            user-name-attribute: sub

pingone:
  application-type: oidc-spa
  registration-id: pingone
  provider-id: pingone
  security:
    post-logout-redirect-uri: https://app.example.com/
```

## Copy-paste: Native `application.yml`

```yaml
pingone:
  application-type: oidc-native
  registration-id: pingone

spring:
  security:
    oauth2:
      client:
        registration:
          pingone:
            client-id: ${PINGONE_CLIENT_ID}
            client-authentication-method: none
            authorization-grant-type: authorization_code
            redirect-uri: http://127.0.0.1:8080/login/oauth2/code/pingone
            scope:
              - openid
              - profile
              - email
            provider: pingone
        provider:
          pingone:
            issuer-uri: https://auth.pingone.com/YOUR-ENV-UUID/as
            user-name-attribute: sub
```

## Copy-paste: env vars

```powershell
$env:PINGONE_APPLICATION_TYPE = "oidc-spa"   # or oidc-native
$env:PINGONE_CLIENT_ID = "your-public-client-id"
$env:PINGONE_ISSUER_URI = "https://auth.pingone.com/YOUR-ENV-UUID/as"
# Do NOT set PINGONE_CLIENT_SECRET for public clients
```

## Registration factory pattern (conceptual Java)

```java
ClientRegistration.withRegistrationId("pingone")
    .clientId(clientId)
    .clientAuthenticationMethod(ClientAuthenticationMethod.NONE)
    .authorizationGrantType(AuthorizationGrantType.AUTHORIZATION_CODE)
    .redirectUri(redirectUri)
    .scope("openid", "profile", "email")
    .userNameAttributeName(IdTokenClaimNames.SUB)
    // + issuer or explicit authorize/token/userinfo/jwks
    .build();
```

## Anti-patterns

- Shipping a client secret inside a SPA bundle
- Using Implicit grant (legacy; prefer Auth Code + PKCE)
- Mismatched redirect URI between Admin console and runtime config

## Next

- [Worker client credentials](worker-client-credentials.md)
- [Spring Boot examples](spring-boot-examples.md)
