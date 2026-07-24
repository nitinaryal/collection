# SCIM copy-paste examples

| Field | Value |
|-------|-------|
| Status | stable |
| Audience | engineers, IGA |
| Level | intermediate–advanced |
| Last reviewed | 2026-07-24 |
| Owner | YankiDev |

**Copyright © 2026 YankiDev ([yankidev.com](https://yankidev.com))**

Replace `YOUR-ENV-UUID`, `WORKER_CLIENT_ID`, `WORKER_CLIENT_SECRET`, and `YOUR-POPULATION-ID` before running.

## 1. Get Worker access token

```bash
export ENV_ID="YOUR-ENV-UUID"
export TOKEN=$(curl -s -X POST "https://auth.pingone.com/${ENV_ID}/as/token" \
  -u "WORKER_CLIENT_ID:WORKER_CLIENT_SECRET" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "grant_type=client_credentials" | jq -r .access_token)

echo "token length: ${#TOKEN}"
```

## 2. Discover schemas

```bash
SCIM="https://api.pingone.com/v1/environments/${ENV_ID}/scim/v2"

curl -s -H "Authorization: Bearer ${TOKEN}" \
  -H "Accept: application/scim+json" \
  "${SCIM}/Schemas" | jq .
```

## 3. Service provider config & resource types

```bash
curl -s -H "Authorization: Bearer ${TOKEN}" \
  -H "Accept: application/scim+json" \
  "${SCIM}/ServiceProviderConfig" | jq .

curl -s -H "Authorization: Bearer ${TOKEN}" \
  -H "Accept: application/scim+json" \
  "${SCIM}/ResourceTypes" | jq .
```

## 4. Create user (Joiner) — standard + enterprise + population

```bash
curl -s -X POST "${SCIM}/Users" \
  -H "Authorization: Bearer ${TOKEN}" \
  -H "Content-Type: application/scim+json" \
  -H "Accept: application/scim+json" \
  -d @- <<'JSON'
{
  "schemas": [
    "urn:ietf:params:scim:schemas:core:2.0:User",
    "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User",
    "urn:pingidentity:schemas:extension:2.0:PingOneUser"
  ],
  "userName": "jdoe@company.com",
  "name": {
    "givenName": "Jane",
    "familyName": "Doe"
  },
  "emails": [
    {
      "value": "jdoe@company.com",
      "type": "work",
      "primary": true
    }
  ],
  "active": true,
  "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User": {
    "employeeNumber": "E12345",
    "department": "Engineering",
    "costCenter": "CC-100"
  },
  "urn:pingidentity:schemas:extension:2.0:PingOneUser": {
    "population": {
      "id": "YOUR-POPULATION-ID"
    }
  }
}
JSON
```

> Exact PingOne extension attribute paths can vary by tenant configuration. Confirm against `GET /Schemas` and your Admin mapping. Population ID is required in most inbound setups.

## 5. Find user by filter

```bash
curl -s -G "${SCIM}/Users" \
  -H "Authorization: Bearer ${TOKEN}" \
  -H "Accept: application/scim+json" \
  --data-urlencode 'filter=userName eq "jdoe@company.com"' | jq .
```

## 6. Mover — PATCH department

```bash
USER_ID="scim-user-id-from-create-response"

curl -s -X PATCH "${SCIM}/Users/${USER_ID}" \
  -H "Authorization: Bearer ${TOKEN}" \
  -H "Content-Type: application/scim+json" \
  -H "Accept: application/scim+json" \
  -d @- <<'JSON'
{
  "schemas": ["urn:ietf:params:scim:api:messages:2.0:PatchOp"],
  "Operations": [
    {
      "op": "replace",
      "path": "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:department",
      "value": "Finance"
    }
  ]
}
JSON
```

## 7. Leaver — deactivate

```bash
curl -s -X PATCH "${SCIM}/Users/${USER_ID}" \
  -H "Authorization: Bearer ${TOKEN}" \
  -H "Content-Type: application/scim+json" \
  -H "Accept: application/scim+json" \
  -d @- <<'JSON'
{
  "schemas": ["urn:ietf:params:scim:api:messages:2.0:PatchOp"],
  "Operations": [
    {
      "op": "replace",
      "path": "active",
      "value": false
    }
  ]
}
JSON
```

## 8. Custom schema block (conceptual)

After defining custom attributes in PingOne and mapping them:

```json
{
  "schemas": [
    "urn:ietf:params:scim:schemas:core:2.0:User",
    "urn:ietf:params:scim:schemas:extension:yourcompany:2.0:User",
    "urn:pingidentity:schemas:extension:2.0:PingOneUser"
  ],
  "userName": "contractor@partner.com",
  "active": true,
  "urn:ietf:params:scim:schemas:extension:yourcompany:2.0:User": {
    "badgeId": "B-9001",
    "securityClearance": "standard"
  },
  "urn:pingidentity:schemas:extension:2.0:PingOneUser": {
    "population": { "id": "YOUR-POPULATION-ID" }
  }
}
```

Confirm the real custom URN from `GET /Schemas` (PingOne may publish `urn:pingidentity:schemas:extension:2.0:User:...` style names for declared attributes).

## 9. Minimal Java sketch (token → SCIM GET)

```java
// 1) Obtain access token via client_credentials (see Worker guide)
// 2) Call SCIM with Bearer token

HttpRequest request = HttpRequest.newBuilder()
    .uri(URI.create("https://api.pingone.com/v1/environments/" + envId + "/scim/v2/Schemas"))
    .header("Authorization", "Bearer " + accessToken)
    .header("Accept", "application/scim+json")
    .GET()
    .build();

HttpResponse<String> response = HttpClient.newHttpClient()
    .send(request, HttpResponse.BodyHandlers.ofString());

System.out.println(response.statusCode());
System.out.println(response.body());
```

## Safety notes

- Never commit Worker secrets or real Population IDs  
- Prefer sandbox env for dry-runs  
- Mask tokens in logs  
- Handle `429 Too Many Requests` with backoff  

## Related

- [SailPoint integration](sailpoint-integration.md)
- [Worker client credentials](../oidc/worker-client-credentials.md)
