# Worker — Client Credentials (copy-paste)

| Field | Value |
|-------|-------|
| Status | stable |
| Audience | engineers, IGA / platform |
| Level | intermediate |
| Last reviewed | 2026-07-24 |
| Owner | YankiDev |

**Copyright © 2026 YankiDev ([yankidev.com](https://yankidev.com))**

Adapted from `WorkerAdoptionGenerator`, `WorkerTokenController`, and SailPoint prep docs in the companion project.

## When to use Worker

| Use Worker | Do not use Worker |
|------------|-------------------|
| SCIM / directory API automation | Interactive user login |
| SailPoint / IGA connectors | Browser SSO |
| Backend jobs calling PingOne APIs | End-user MFA journeys |

Worker apps authenticate with **client_credentials**, receive an **access token**, and call APIs with `Authorization: Bearer ...`.

## PingOne Admin checklist

- [ ] Application type: **Worker**
- [ ] Grant: **Client Credentials**
- [ ] Assign roles such as **Identity Data Admin**, **Identity Router**, or **User Admin** (as required)
- [ ] No redirect URI
- [ ] Store Client ID + Secret securely; plan secret rotation

## Copy-paste: `application.yml`

```yaml
pingone:
  application-type: worker
  registration-id: pingone-worker

spring:
  security:
    oauth2:
      client:
        registration:
          pingone-worker:
            client-id: ${PINGONE_CLIENT_ID}
            client-secret: ${PINGONE_CLIENT_SECRET}
            client-authentication-method: client_secret_basic
            authorization-grant-type: client_credentials
            scope:
              - openid
            provider: pingone-worker
        provider:
          pingone-worker:
            issuer-uri: https://auth.pingone.com/YOUR-ENV-UUID/as
```

## Copy-paste: env vars

```powershell
$env:PINGONE_APPLICATION_TYPE = "worker"
$env:PINGONE_REGISTRATION_ID = "pingone-worker"
$env:PINGONE_CLIENT_ID = "worker-client-id"
$env:PINGONE_CLIENT_SECRET = "worker-client-secret"
$env:PINGONE_ISSUER_URI = "https://auth.pingone.com/YOUR-ENV-UUID/as"
```

## Copy-paste: obtain a token with curl

```bash
curl -s -X POST "https://auth.pingone.com/YOUR-ENV-UUID/as/token" \
  -u "WORKER_CLIENT_ID:WORKER_CLIENT_SECRET" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "grant_type=client_credentials"
```

Response (shape):

```json
{
  "access_token": "eyJ...",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

## Copy-paste: Spring — acquire token in code

```java
@GetMapping("/worker/token")
@ResponseBody
public Map<String, Object> token(
        PingOneClientProperties properties,
        OAuth2AuthorizedClientManager authorizedClientManager) {

    OAuth2AuthorizeRequest authorizeRequest = OAuth2AuthorizeRequest
            .withClientRegistrationId(properties.getRegistrationId())
            .principal("pingone-worker-service")
            .build();

    OAuth2AuthorizedClient client = authorizedClientManager.authorize(authorizeRequest);
    if (client == null || client.getAccessToken() == null) {
        throw new IllegalStateException("Failed to obtain worker access token");
    }

    return Map.of(
            "tokenType", client.getAccessToken().getTokenType().getValue(),
            "expiresAt", client.getAccessToken().getExpiresAt(),
            "scopes", client.getAccessToken().getScopes()
            // mask token in logs / UI — never log full secrets
    );
}
```

Ensure your `OAuth2AuthorizedClientProvider` includes `.clientCredentials()` (the companion starter registers authorization code + refresh + client credentials).

## Bridge to SCIM

```text
Worker token  -->  Authorization: Bearer <access_token>
              -->  https://api.pingone.com/v1/environments/{env-id}/scim/v2/Users
```

Continue in [SCIM copy-paste examples](../scim/copy-paste-examples.md).

## Next

- [SCIM SailPoint integration](../scim/sailpoint-integration.md)
- [Discovery, JWKS, logout](discovery-jwks-logout.md)
