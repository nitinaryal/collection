# OIDC Web App — Authorization Code (copy-paste)

| Field | Value |
|-------|-------|
| Status | stable |
| Audience | engineers |
| Level | intro–intermediate |
| Last reviewed | 2026-07-24 |
| Owner | YankiDev |

**Copyright © 2026 YankiDev ([yankidev.com](https://yankidev.com))**

Adapted from the companion project’s OIDC Web App adoption artifacts and `application.yml`.

## PingOne Admin checklist

- [ ] Application type: **OIDC Web App**
- [ ] Grant type: **Authorization Code**
- [ ] Redirect URI exactly: `http://localhost:8080/login/oauth2/code/pingone` (or your host/path)
- [ ] Post-logout redirect URI: `http://localhost:8080/`
- [ ] Scopes enabled: `openid`, `profile`, `email`
- [ ] Copy **Client ID** and **Client Secret**
- [ ] Note issuer: `https://auth.pingone.com/<your-env-uuid>/as`

**Critical rule:** Spring registration key must equal `pingone.registration-id`. Redirect path is always:

```text
/login/oauth2/code/{registration-id}
```

If registration id is `acme-pingone`, redirect becomes `/login/oauth2/code/acme-pingone` — update PingOne Admin to match.

## Copy-paste: `application.yml` (local)

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
            client-secret: ${PINGONE_CLIENT_SECRET}
            client-authentication-method: client_secret_basic
            authorization-grant-type: authorization_code
            redirect-uri: http://localhost:8080/login/oauth2/code/pingone
            scope:
              - openid
              - profile
              - email
            provider: pingone
        provider:
          pingone:
            issuer-uri: ${PINGONE_ISSUER_URI}
            user-name-attribute: sub

pingone:
  application-type: oidc-web-app
  registration-id: pingone
  provider-id: pingone
  ui:
    post-login-path: /dashboard
  security:
    post-logout-redirect-uri: http://localhost:8080/
  metadata:
    discovery-document-path: /.well-known/openid-configuration
```

## Copy-paste: environment variables (PowerShell)

```powershell
$env:PINGONE_APPLICATION_TYPE = "oidc-web-app"
$env:PINGONE_REGISTRATION_ID = "pingone"
$env:PINGONE_PROVIDER_ID = "pingone"
$env:PINGONE_CLIENT_ID = "your-client-id"
$env:PINGONE_CLIENT_SECRET = "your-client-secret"
$env:PINGONE_ISSUER_URI = "https://auth.pingone.com/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/as"
```

## Copy-paste: production-style rename

```yaml
pingone:
  application-type: oidc-web-app
  registration-id: acme-pingone
  security:
    post-logout-redirect-uri: https://app.acme.com/

spring:
  security:
    oauth2:
      client:
        registration:
          acme-pingone:
            client-id: ${ACME_PINGONE_CLIENT_ID}
            client-secret: ${ACME_PINGONE_CLIENT_SECRET}
            redirect-uri: https://app.acme.com/login/oauth2/code/acme-pingone
            scope:
              - openid
              - profile
              - email
            provider: acme-pingone
        provider:
          acme-pingone:
            issuer-uri: https://auth.pingone.com/YOUR-ENV-UUID/as
            user-name-attribute: sub
```

## Option B — explicit endpoints (no issuer discovery at startup)

Useful for mocks or locked-down networks:

```yaml
spring:
  security:
    oauth2:
      client:
        provider:
          pingone:
            # leave issuer-uri empty / unset
            authorization-uri: https://auth.pingone.com/YOUR-ENV-UUID/as/authorize
            token-uri: https://auth.pingone.com/YOUR-ENV-UUID/as/token
            user-info-uri: https://auth.pingone.com/YOUR-ENV-UUID/as/userinfo
            jwk-set-uri: https://auth.pingone.com/YOUR-ENV-UUID/as/jwks
            user-name-attribute: sub
```

## Runtime URLs (Spring Security defaults)

| Action | URL |
|--------|-----|
| Start login | `GET /oauth2/authorization/pingone` |
| Callback | `GET /login/oauth2/code/pingone` |
| Logout | `POST /logout` (with CSRF as configured) |

## Maven dependencies

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-security</artifactId>
</dependency>
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-oauth2-client</artifactId>
</dependency>
```

## Next

- [Spring Boot examples](spring-boot-examples.md)
- [Discovery, JWKS, logout](discovery-jwks-logout.md)
