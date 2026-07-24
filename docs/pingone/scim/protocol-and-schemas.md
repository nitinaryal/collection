# SCIM protocol and schemas

| Field | Value |
|-------|-------|
| Status | stable |
| Audience | engineers, architects |
| Level | intermediate |
| Last reviewed | 2026-07-24 |
| Owner | YankiDev |

**Copyright © 2026 YankiDev ([yankidev.com](https://yankidev.com))**

## Terminology hierarchy (use these words in design reviews)

| Term | Meaning |
|------|---------|
| **Resource** | One identity object instance (e.g. Jane Doe) with `id`, `externalId`, `meta` |
| **Schema** | Attribute blueprint + types + mutability, identified by a **URN** |
| **Resource Type** | Maps an endpoint (e.g. `/Users`) to a core schema + allowed extensions |

## Core schemas (RFC 7643)

| URN | Purpose |
|-----|---------|
| `urn:ietf:params:scim:schemas:core:2.0:User` | `userName`, `name`, `emails`, `active`, … |
| `urn:ietf:params:scim:schemas:core:2.0:Group` | `displayName`, `members`, … |
| `urn:ietf:params:scim:schemas:extension:enterprise:2.0:User` | `employeeNumber`, `costCenter`, `department`, `manager`, … |

## Custom schema extensions

Custom attributes must live under a **namespace URN**, not loose root fields:

```text
urn:ietf:params:scim:schemas:extension:yourcompany:2.0:User
```

Types must still be SCIM primitives (String, Boolean, Integer, Decimal, DateTime, Binary, Reference). Advertise extensions via `GET /Schemas`.

## Standard endpoints (RFC 7644)

| Method + path | Action |
|---------------|--------|
| `POST /Users` | Create |
| `GET /Users/{id}` | Read |
| `PUT /Users/{id}` | Full replace |
| `PATCH /Users/{id}` | Partial update (`add` / `remove` / `replace`) |
| `DELETE /Users/{id}` | Delete |
| `GET /Users?filter=...` | Search |
| `POST /Bulk` | Batch operations |
| `GET /Schemas` | Discover schemas |
| `GET /ResourceTypes` | Discover resource types |
| `GET /ServiceProviderConfig` | Feature support (PATCH, bulk, …) |

### Filter examples

```http
GET /Users?filter=userName eq "jdoe@company.com"
GET /Users?filter=name.familyName sw "Smi"
```

Common operators: `eq`, `co`, `sw`, `ew`, `pr`, `gt`, `ge`, `lt`, `le`, and logical `and` / `or`.

## Discovery objects

| Endpoint | Tells the client |
|----------|------------------|
| `/ServiceProviderConfig` | PATCH? bulk? sort? max operations? |
| `/ResourceTypes` | Which resources exist |
| `/Schemas` | Exact attributes and types |

Always discover before hard-coding assumptions in connectors.

## Next

- [PingOne SCIM API](pingone-scim-api.md)
- [Copy-paste examples](copy-paste-examples.md)
