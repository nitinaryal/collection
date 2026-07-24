# Spring Boot OIDC examples (advanced)

| Field | Value |
|-------|-------|
| Status | stable |
| Audience | Java / Spring engineers |
| Level | intermediate–advanced |
| Last reviewed | 2026-07-24 |
| Owner | YankiDev |

**Copyright © 2026 YankiDev ([yankidev.com](https://yankidev.com))**

Patterns below mirror the companion modules:

- `pingone-oidc-spring-boot-starter`
- `pingone-oidc-test-client`

## Security filter chain pattern

```java
http.authorizeHttpRequests(auth ->
        auth.requestMatchers(publicPaths).permitAll()
            .anyRequest().authenticated());

// Resolve configurer by pingone.application-type
configurerFactory.resolve().configure(http, properties, clientRegistrationRepository);
```

## Interactive OIDC (`oauth2Login`)

```java
http.oauth2Login(oauth2 -> {
    oauth2.clientRegistrationRepository(clientRegistrationRepository);
    // optional success/failure handlers
    oauth2.defaultSuccessUrl("/dashboard", true);
});

http.logout(logout -> {
    // optional: OidcClientInitiatedLogoutSuccessHandler / custom end-session handler
});
```

## Confidential ClientRegistration (Web App)

```java
ClientRegistration.withRegistrationId(registrationId)
    .clientId(clientId)
    .clientSecret(clientSecret)
    .clientAuthenticationMethod(ClientAuthenticationMethod.CLIENT_SECRET_BASIC)
    .authorizationGrantType(AuthorizationGrantType.AUTHORIZATION_CODE)
    .redirectUri(redirectUri)
    .scope("openid", "profile", "email")
    .userNameAttributeName(IdTokenClaimNames.SUB)
    // apply issuer-uri OR explicit authorize/token/userinfo/jwks
    .build();
```

## Authorized client manager (multiple grants)

```java
OAuth2AuthorizedClientProvider provider = OAuth2AuthorizedClientProviderBuilder.builder()
        .authorizationCode()
        .refreshToken()
        .clientCredentials()
        .build();
```

## Mock mode (learn offline)

Companion project embeds a mini OP under `/mock/pingone/as` when started with `-Dmock=true`:

```powershell
mvn spring-boot:run -pl pingone-oidc-test-client -am -Dmock=true
```

| Mock setting | Value |
|--------------|--------|
| Issuer | `http://localhost:{port}/mock/pingone/as` |
| Client ID | `mock-client-id` |
| Client secret | `mock-client-secret` |

Open the Client Integration Tool at `/tool` to generate adoption YAML, run diagnostics, and walk login/logout/claims tests.

## Real mode env (PowerShell)

```powershell
$env:PINGONE_ISSUER_URI = "https://auth.pingone.com/<your-env-uuid>/as"
$env:PINGONE_CLIENT_ID = "your-client-id"
$env:PINGONE_CLIENT_SECRET = "your-client-secret"
mvn spring-boot:run -pl pingone-oidc-test-client -am
```

## Useful endpoints in the test client

| Endpoint | Description |
|----------|-------------|
| `/` | Home |
| `/tool` | Integration wizard (public) |
| `/dashboard` | Post-login landing |
| `/me` | ID Token claims |
| `/token` | Access token (masked) |
| `/jwks` | JWKS |
| `/metadata` | Discovery document |
| `/worker/token` | Client-credentials demo |
| `/oauth2/authorization/{registrationId}` | Start OIDC login |

## Next

- [Reference: source project](../reference-source-project.md)
- [SAML track](../saml/README.md)
