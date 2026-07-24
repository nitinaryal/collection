# Discovery, JWKS, and logout

| Field | Value |
|-------|-------|
| Status | stable |
| Audience | engineers |
| Level | intermediate |
| Last reviewed | 2026-07-24 |
| Owner | YankiDev |

**Copyright © 2026 YankiDev ([yankidev.com](https://yankidev.com))**

## Discovery document

```http
GET {issuer}/.well-known/openid-configuration
```

Example issuer:

```text
https://auth.pingone.com/YOUR-ENV-UUID/as
```

### Copy-paste: fetch with curl

```bash
curl -s "https://auth.pingone.com/YOUR-ENV-UUID/as/.well-known/openid-configuration" | jq .
```

Validate at least:

- `issuer` matches what you configured (no leftover `{env-id}` placeholders)
- `authorization_endpoint`, `token_endpoint`, `jwks_uri`, `end_session_endpoint` present
- `grant_types_supported` includes what your app needs (`authorization_code`, `client_credentials`, …)

## JWKS — validating ID Tokens

```http
GET {jwks_uri}
```

Your library loads keys and verifies:

| Claim | Check |
|-------|--------|
| `iss` | Equals configured issuer |
| `aud` | Contains your `client_id` |
| `exp` / `iat` | Not expired; reasonable clock skew |
| `signature` | Matches a key from JWKS |

### Companion test-client endpoints

When running the pingone test client:

| Path | Purpose |
|------|---------|
| `/metadata` | Show discovery document |
| `/jwks` | Fetch / display JWKS |
| `/me` | Show selected ID Token claims |
| `/token` | Show masked access token info |

## RP-initiated logout

1. App clears local session (`POST /logout`).
2. Optionally redirect browser to PingOne `end_session_endpoint` with `id_token_hint` and `post_logout_redirect_uri`.
3. `post_logout_redirect_uri` **must** be registered in PingOne Admin (exact match).

### Config property (companion template)

```yaml
pingone:
  security:
    post-logout-redirect-uri: http://localhost:8080/
```

## Common failures

| Symptom | Likely cause |
|---------|----------------|
| `issuer-uri` parse / template errors | Literal `{environment-id}` left in YAML |
| `redirect_uri_mismatch` | Admin URI ≠ runtime URI (incl. port / registration-id) |
| Invalid signature | Wrong JWKS / wrong issuer environment |
| Logout returns to wrong host | Post-logout URI not registered |

## Next

- [Spring Boot examples](spring-boot-examples.md)
- [Source project](../reference-source-project.md)
