# SCIM with PingOne

SCIM (System for Cross-domain Identity Management) is how enterprise IGA tools provision and deprovision identities into PingOne. This track covers RFC concepts, PingOne’s SCIM gateway, JML use cases, SailPoint prep, and copy-paste HTTP examples.

**Copyright © 2026 YankiDev ([yankidev.com](https://yankidev.com))**

Source material adapted from companion project docs: `SCIM.docx`, `SailPoint_PingOne_Integration_Prep.docx`, `PINGONE_SCIM_GUIDE.pdf`, and architecture notes.

## Contents

| Document | Level | Focus |
|----------|-------|--------|
| [SCIM basics](basics.md) | intro | Why SCIM, RFCs, philosophy |
| [Protocol & schemas](protocol-and-schemas.md) | intermediate | Endpoints, URNs, standard vs custom |
| [PingOne SCIM API](pingone-scim-api.md) | intermediate | Base URL, native vs SCIM, population |
| [JML use cases](jml-use-cases.md) | intermediate | Joiner / Mover / Leaver / certification |
| [SailPoint integration](sailpoint-integration.md) | advanced | Worker roles, prep phases, ops |
| [Copy-paste examples](copy-paste-examples.md) | intermediate–advanced | Token + Users + PATCH payloads |

## One-sentence mental model

> **An identity is an identity, no matter where it lives** — SCIM standardizes the JSON model and the REST verbs so HR/IGA systems and PingOne speak the same contract.

## Prerequisite

SCIM calls to PingOne are authenticated with a **Worker** Bearer token. Complete [Worker client credentials](../oidc/worker-client-credentials.md) first.
