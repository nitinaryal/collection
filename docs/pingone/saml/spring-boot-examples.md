# SAML Spring Boot examples (copy-paste)

| Field | Value |
|-------|-------|
| Status | stable |
| Audience | Java / Spring engineers |
| Level | intermediate |
| Last reviewed | 2026-07-24 |
| Owner | YankiDev |

**Copyright © 2026 YankiDev ([yankidev.com](https://yankidev.com))**

Adapted from the companion project’s SAML adoption generator and `samples/saml_index.html`. Full `SamlSecurityConfigurer` is planned in that template; the YAML below is the adoption artifact you can copy into a Spring Security SAML2 app.

## PingOne Admin checklist

- [ ] Application type: **SAML**
- [ ] Configure SP metadata (Entity ID + ACS URL)
- [ ] Map SAML attributes to application user fields
- [ ] Download / use IdP metadata URL
- [ ] No OIDC redirect URI required

## Copy-paste: `application.yml`

```yaml
pingone:
  application-type: saml

spring:
  security:
    saml2:
      relyingparty:
        registration:
          pingone:
            entity-id: https://app.example.com/saml/metadata
            assertingparty:
              metadata-uri: https://auth.pingone.com/YOUR-ENV-UUID/saml20/metadata
            acs:
              location: https://app.example.com/login/saml2/sso/pingone
```

Replace:

| Placeholder | With |
|-------------|------|
| `YOUR-ENV-UUID` | Real environment id |
| `entity-id` | Your SP entity ID (must match PingOne) |
| `acs.location` | Your ACS URL (must match PingOne) |

## Maven dependency

```xml
<dependency>
  <groupId>org.springframework.security</groupId>
  <artifactId>spring-security-saml2-service-provider</artifactId>
</dependency>
```

## Implementation checklist

1. Add `spring-security-saml2-service-provider`
2. Import PingOne SAML metadata (`metadata-uri`)
3. Register ACS URL and entity ID in PingOne Admin
4. Map assertion attributes → local principal
5. Test: `/saml2/authenticate/pingone` → PingOne login → ACS → session

## Copy-paste: assertion display page (Thymeleaf)

From companion `samples/saml_index.html` — useful for POC attribute debugging:

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>SAML 2.0 POC Application</title>
</head>
<body>
<div>
    <h1>SAML 2.0 Connection Successful!</h1>
    <p>Logged in User Identifier:
       <strong th:text="${username}">user@company.com</strong></p>

    <h2>SAML Assertions Received from PingOne</h2>
    <table>
        <thead>
            <tr><th>Attribute Name</th><th>Attribute Value(s)</th></tr>
        </thead>
        <tbody>
            <tr th:each="entry : ${attributes}">
                <td th:text="${entry.key}">Attribute Name</td>
                <td>
                    <ul>
                        <li th:each="val : ${entry.value}" th:text="${val}">Value</li>
                    </ul>
                </td>
            </tr>
        </tbody>
    </table>
</div>
</body>
</html>
```

Wire `${username}` and `${attributes}` from your SAML authentication success handler / controller.

## Test flow (teaching)

1. SP initiates SSO — user hits `/saml2/authenticate/{registrationId}`
2. PingOne authenticates the user
3. SAML assertion posted to ACS
4. App validates and creates a session

## Next

- [Protocols comparison](../protocols-comparison.md)
- [OIDC Web App](../oidc/authorization-code-web-app.md) if you can choose OIDC instead
