# Reference — companion `pingone` source project

| Field | Value |
|-------|-------|
| Status | stable |
| Audience | engineers using this Collection with the hands-on repo |
| Level | intermediate |
| Last reviewed | 2026-07-24 |
| Owner | YankiDev |

**Copyright © 2026 YankiDev ([yankidev.com](https://yankidev.com))**

This Collection’s PingOne docs are curated from the companion repository (local path often `C:\nitin\projects\pingone`). Use that project for runnable demos; use **this** folder for learning and copy-paste references.

## Modules

| Module | Role |
|--------|------|
| `pingone-oidc-spring-boot-starter` | Reusable Spring Boot auto-config, registration factories, security configurers |
| `pingone-oidc-test-client` | Runnable UI + `/tool` wizard + mock OP + adoption artifact generators |

## Docs & materials in the source repo

| Asset | Used for |
|-------|----------|
| `README.md` | Run instructions, Admin table, config reference |
| `SCIM.docx` / `PINGONE_SCIM_GUIDE.pdf` | SCIM RFC + PingOne inbound concepts |
| `SailPoint_PingOne_Integration_Prep.docx` | Worker + SCIM operational prep |
| `pingone-oidc-spring-boot-starter/docs/LDAP_Sailpoint_SCIM_Protocol_PingOne_Architecutre.txt` | Architecture ASCII diagram |
| `samples/saml_index.html` | SAML assertion display POC |
| `login.json` | Sample DaVinci login flow export |

## How to run (from source repo root)

### Mock (no tenant)

```powershell
mvn spring-boot:run -pl pingone-oidc-test-client -am -Dmock=true
# or .\run.ps1
```

Open `http://localhost:8080/tool` (or the configured port).

### Real PingOne

```powershell
$env:PINGONE_ISSUER_URI = "https://auth.pingone.com/<your-env-uuid>/as"
$env:PINGONE_CLIENT_ID = "your-client-id"
$env:PINGONE_CLIENT_SECRET = "your-client-secret"
mvn spring-boot:run -pl pingone-oidc-test-client -am
# or .\run-real.ps1
```

## Map: Collection doc → source code

| Collection topic | Source highlights |
|------------------|-------------------|
| OIDC Web App YAML | `OidcWebAppAdoptionGenerator`, `application.yml` |
| SPA/Native PKCE | `OidcPublicClientAdoptionGenerator`, public registration factories |
| Worker token | `WorkerAdoptionGenerator`, `WorkerTokenController` |
| Security `oauth2Login` | `OidcInteractiveSecurityConfigurerBase` |
| SAML YAML | `SamlAdoptionGeneratorBean` in `DeviceAdoptionGenerator.java` |
| App-type catalog | `PingOneApplicationTypeCatalog` |
| Mock OP | `MockPingOneOidcController` and related mock package |
| SCIM / SailPoint | Root Word/PDF docs + architecture txt (no Java SCIM client) |

## Maturity note

| Type | In companion code |
|------|-------------------|
| OIDC Web App | Fully runnable |
| SPA / Native / Worker | Config + factories / partial UI support |
| Device / SAML | Catalog + adoption YAML / samples; full security configurers planned |
| SCIM HTTP client | Documented; use curl/Bruno/SailPoint against PingOne APIs |

## Related

- [Learning path](learning-path.md)
- [PingOne index](README.md)
