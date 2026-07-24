# PingOne SCIM API

| Field | Value |
|-------|-------|
| Status | stable |
| Audience | engineers, IGA |
| Level | intermediate |
| Last reviewed | 2026-07-24 |
| Owner | YankiDev |

**Copyright © 2026 YankiDev ([yankidev.com](https://yankidev.com))**

## Base URL

```text
https://api.pingone.com/v1/environments/{env-id}/scim/v2
```

Examples:

```text
https://api.pingone.com/v1/environments/{env-id}/scim/v2/Users
https://api.pingone.com/v1/environments/{env-id}/scim/v2/Groups
https://api.pingone.com/v1/environments/{env-id}/scim/v2/Schemas
https://api.pingone.com/v1/environments/{env-id}/scim/v2/ResourceTypes
https://api.pingone.com/v1/environments/{env-id}/scim/v2/ServiceProviderConfig
```

## Authentication

1. Create a **Worker** application with Client Credentials  
2. Assign directory roles (e.g. Identity Data Admin)  
3. `POST` to `https://auth.pingone.com/{env-id}/as/token`  
4. Call SCIM with `Authorization: Bearer <access_token>`

See [Worker guide](../oidc/worker-client-credentials.md).

## Population is mandatory on create

PingOne partitions users into **Populations**. Inbound creates typically must include:

```text
urn:pingidentity:schemas:extension:2.0:PingOneUser:population.id
```

Provide SailPoint (or your SCIM client) the target **Population ID**.

## Native API vs SCIM gateway

| | Native `.../v1/...` | SCIM `.../scim/v2/...` |
|--|--------------------|------------------------|
| Payload style | PingOne HAL (`_links`, `_embedded`) | Standard SCIM JSON + URNs |
| Best for | Admin UI / custom tools | SailPoint, Entra, Okta, portable IGA |
| Schema discovery | Platform-specific | `/Schemas` |

## Inbound provisioning in PingOne Admin (custom attributes)

1. **Identities → Attributes** — declare custom attributes  
2. **Connections → Provisioning** — SCIM connection; register custom schema URNs  
3. **Attribute mapping** — map inbound SCIM paths → PingOne directory attributes  

### Standard enterprise extension

PingOne already understands `urn:ietf:params:scim:schemas:extension:enterprise:2.0:User`. You still **map** fields (e.g. SailPoint `Cost_Center_Code` → `costCenter`); you do not redefine the schema.

### Custom attributes

1. Define in PingOne first (appears under a PingOne extension URN)  
2. Map in the SCIM client so payloads include that URN block  

## Next

- [JML use cases](jml-use-cases.md)
- [SailPoint integration](sailpoint-integration.md)
- [Copy-paste examples](copy-paste-examples.md)
