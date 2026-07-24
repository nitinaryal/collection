# SCIM basics

| Field | Value |
|-------|-------|
| Status | stable |
| Audience | engineers, IGA practitioners |
| Level | intro |
| Last reviewed | 2026-07-24 |
| Owner | YankiDev |

**Copyright © 2026 YankiDev ([yankidev.com](https://yankidev.com))**

## What problem SCIM solves

Before SCIM, every cloud app invented a proprietary user-management API. Automating Joiner/Mover/Leaver across dozens of systems meant custom connectors for each target.

**SCIM** (IETF) standardizes:

1. **JSON object models** for users and groups  
2. **RESTful HTTP** operations to create, read, update, delete those objects  

## RFC map

| RFC | Role |
|-----|------|
| **RFC 7642** | Concepts, scenarios, vocabulary |
| **RFC 7643** | Core schema (attributes, types, extensions) |
| **RFC 7644** | Protocol (REST paths, filters, bulk, status codes) |

## Where PingOne fits

PingOne exposes an **inbound SCIM 2.0** gateway. External IGA platforms (commonly **SailPoint** with a Universal SCIM connector) manage directory lifecycle states without writing PingOne-proprietary CRUD for every attribute.

## Business use cases beyond JML

| Use case | Idea |
|----------|------|
| **B2B SaaS onboarding** | Customer IdP provisions accounts into your product via SCIM |
| **M&A consolidation** | Push identities from acquired company’s IdP into PingOne |
| **Partner / vendor access** | Partner manages contractors; SCIM deactivates when they leave |
| **Org chart / group sync** | Keep `/Groups` and membership consistent for RBAC |

## Next

- [Protocol & schemas](protocol-and-schemas.md)
- [JML use cases](jml-use-cases.md)
