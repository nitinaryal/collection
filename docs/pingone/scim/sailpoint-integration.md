# SailPoint ↔ PingOne integration

| Field | Value |
|-------|-------|
| Status | stable |
| Audience | IGA engineers, platform owners |
| Level | advanced |
| Last reviewed | 2026-07-24 |
| Owner | YankiDev |

**Copyright © 2026 YankiDev ([yankidev.com](https://yankidev.com))**

Adapted from companion `SailPoint_PingOne_Integration_Prep.docx`.

## Why Worker (not OIDC Web App / SAML)

SailPoint is an **administrative system**, not an end user. Worker is the PingOne application type that:

- Holds platform administrative roles  
- Authenticates non-interactively with **client credentials**  
- Can write directory data needed for SCIM provisioning  

Interactive OIDC/SAML apps cannot replace this.

## Phase 1 — Prerequisites

1. **Create Worker application** — Client Credentials grant  
2. **Assign roles** — Identity Data Admin / Identity Router / User Admin (as required)  
3. **Share credentials securely** — Client ID + Client Secret  
4. **Token URL** — `https://auth.pingone.com/{envID}/as/token`  
5. **SCIM base URL** — `https://api.pingone.com/v1/environments/{envID}/scim/v2`  
6. **Default Population ID** — required on user create (`urn:pingidentity:schemas:extension:2.0:PingOneUser:population.id`)  
7. **Schema mapping docs** — document custom attributes under Identities → Attributes  

## Phase 2 — Dry-run before connecting SailPoint

Validate with Bruno / curl / Postman:

1. Obtain token (client credentials)  
2. `GET .../scim/v2/Schemas`  
3. `GET .../scim/v2/ServiceProviderConfig`  
4. `GET .../scim/v2/ResourceTypes`  
5. Dry-run `POST .../scim/v2/Users` with dummy data  

Endpoints commonly required by SailPoint connectors:

- `/Users`  
- `/Groups` (if used)  
- `/ServiceProviderConfig`  
- `/ResourceTypes`  
- `/Schemas`  
- `/ResourceTypes/User`  

Then share sandbox **ClientId**, **Secret**, **PopulationId**, **EnvironmentId** with the SailPoint team.

## Phase 3 — Operational readiness

| Concern | Practice |
|---------|----------|
| **Rate limits** | Watch 429s; SailPoint bulk can hammer APIs |
| **Webhooks / audit** | Subscribe directory events → SIEM (Splunk, Datadog, …) |
| **Dashboards** | Track User.Create / Update / Delete by Worker client id |
| **Secret rotation** | Alarm before Worker secret expiry; coordinate with IGA team |

## Architecture reminder

See [Architecture](../03-architecture.md) for AD → SailPoint → Worker + SCIM → Environment.

## Next

- [Copy-paste examples](copy-paste-examples.md)
- [Worker OIDC](../oidc/worker-client-credentials.md)
