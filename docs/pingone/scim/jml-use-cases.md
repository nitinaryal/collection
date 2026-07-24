# JML use cases (Joiner / Mover / Leaver)

| Field | Value |
|-------|-------|
| Status | stable |
| Audience | architects, IGA, security |
| Level | intermediate |
| Last reviewed | 2026-07-24 |
| Owner | YankiDev |

**Copyright © 2026 YankiDev ([yankidev.com](https://yankidev.com))**

Enterprise SCIM integrations with PingOne are usually justified by lifecycle automation.

## Use Case A — Joiner (birthright provisioning)

| | |
|--|--|
| **Scenario** | New engineer hired in HR |
| **Action** | IGA aggregates HR data → `POST /Users` to PingOne SCIM |
| **Result** | User created in correct **Population**; can SSO on day one |

## Use Case B — Mover (dynamic access change)

| | |
|--|--|
| **Scenario** | Employee moves Marketing → Finance |
| **Action** | `PATCH /Users/{id}` updates department / group memberships |
| **Result** | Access via PingOne SSO reflects new role immediately |

## Use Case C — Leaver (offboarding)

| | |
|--|--|
| **Scenario** | Employee terminated |
| **Action** | High-priority `PATCH` sets `"active": false` (or equivalent disable) |
| **Result** | Account suspended; SSO sessions revoked; resources locked out |

## Use Case D — Access certification

| | |
|--|--|
| **Scenario** | Quarterly review of privileged access |
| **Action** | IGA reads `/Users` and `/Groups` from PingOne SCIM |
| **Result** | Managers approve/revoke; compliance evidence retained |

## Upstream → IGA → PingOne pipeline

```text
HR / AD  --aggregate-->  SailPoint (policy, SoD)
                     --SCIM POST/PATCH-->  PingOne Directory
                     --OIDC/SAML SSO-->    Applications
```

## Next

- [SailPoint integration](sailpoint-integration.md)
- [Copy-paste examples](copy-paste-examples.md)
