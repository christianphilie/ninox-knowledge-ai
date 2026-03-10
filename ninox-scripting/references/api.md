# Ninox API Reference

## Overview

Ninox provides two ways to interact with APIs:
1. **HTTP from Ninox** — Making outgoing HTTP requests from NX scripts using `http()`
2. **Ninox REST API** — Accessing your Ninox database from external systems

**Source**: https://docs.ninox.com/en/api, https://docs.ninox.com/en/script/http-requests

---

## Part 1: Making HTTP Requests FROM Ninox Scripts

See `references/http-requests.md` for full documentation.

### Quick Reference
```ninox
"⚠️ CORRECT http() signature: method is FIRST parameter"
let response := do as server
    http("POST", "https://api.example.com/endpoint",
        {
            "Content-Type": "application/json",
            "Authorization": "Bearer TOKEN"
        },
        {key: "value"}
    )
end;
if response.error then
    alert(text(response.error))
else
    "Use response.result"
end
```

---

## Part 2: Ninox REST API (External Access)

### Base URLs

| Environment | Base URL |
|---|---|
| Ninox Cloud (Public) | `https://api.ninox.com/v1` |
| Ninox Cloud (Legacy URL) | `https://api.ninoxdb.de/v1` (still functional) |
| Private Cloud | `https://{yoursubdomain}.ninoxdb.de/v1/` |
| On-Premises | Custom domain (e.g., `https://ninox.mycompany.com/v1/`) or IP:Port |

**Note**: Ninox migrated from `ninoxdb.de` to `ninox.com` for the public cloud. The old `api.ninoxdb.de/v1` interface still works but `api.ninox.com/v1` is current. Private Cloud instances use `.ninoxdb.de` subdomains unless a custom domain is configured.

### Authentication

All requests require an API key in the Authorization header:

```
Authorization: Bearer YOUR_NINOX_API_KEY
```

**Getting Your API Key**:
1. Log into Ninox
2. Go to Settings → Integrations (or Profile → API)
3. Generate or copy your Personal API Key

---

## REST API Endpoints

### Teams / Workspaces

#### List Teams
```
GET /v1/teams
```

```bash
curl -H "Authorization: Bearer YOUR_API_KEY" \
  https://api.ninox.com/v1/teams
```

**Response**:
```json
[
  {
    "id": "team_abc123",
    "name": "My Company"
  }
]
```

---

### Databases

#### List Databases in a Team
```
GET /v1/teams/{teamId}/databases
```

```bash
curl -H "Authorization: Bearer YOUR_API_KEY" \
  "https://api.ninox.com/v1/teams/team_abc123/databases"
```

**Response**:
```json
[
  {"id": "db_001", "name": "CRM"},
  {"id": "db_002", "name": "Inventory"}
]
```

---

### Tables

#### List Tables in a Database
```
GET /v1/teams/{teamId}/databases/{databaseId}/tables
```

```bash
curl -H "Authorization: Bearer YOUR_API_KEY" \
  "https://api.ninox.com/v1/teams/TEAM_ID/databases/DB_ID/tables"
```

**Response**:
```json
[
  {"id": "tbl_a", "name": "Customers"},
  {"id": "tbl_b", "name": "Orders"}
]
```

#### Get Table Fields (Schema)
```
GET /v1/teams/{teamId}/databases/{databaseId}/tables/{tableId}/fields
```

```bash
curl -H "Authorization: Bearer YOUR_API_KEY" \
  "https://api.ninox.com/v1/teams/TEAM_ID/databases/DB_ID/tables/TABLE_ID/fields"
```

**Response**:
```json
[
  {"id": "A", "name": "Name", "type": "text"},
  {"id": "B", "name": "Status", "type": "choice", "choices": ["Active", "Inactive"]},
  {"id": "C", "name": "Amount", "type": "number"},
  {"id": "D", "name": "Customer", "type": "reference", "referenceTable": "tbl_a"}
]
```

---

### Records

#### Get All Records
```
GET /v1/teams/{teamId}/databases/{databaseId}/tables/{tableId}/records
```

```bash
curl -H "Authorization: Bearer YOUR_API_KEY" \
  "https://api.ninox.com/v1/teams/TEAM_ID/databases/DB_ID/tables/TABLE_ID/records"
```

**Query Parameters**:
| Parameter | Description | Example |
|---|---|---|
| `page` | Page number (pagination) | `?page=1` |
| `pageSize` | Records per page | `?pageSize=100` |

**Response**:
```json
{
  "data": [
    {
      "id": 1,
      "sequence": 1,
      "createdAt": "2024-03-15T10:30:00.000Z",
      "updatedAt": "2024-03-15T14:00:00.000Z",
      "fields": {
        "Name": "Acme Corp",
        "Status": "Active",
        "Amount": 5000
      }
    }
  ],
  "totalCount": 1
}
```

#### Get Single Record
```
GET /v1/teams/{teamId}/databases/{databaseId}/tables/{tableId}/records/{recordId}
```

#### Create Record
```
POST /v1/teams/{teamId}/databases/{databaseId}/tables/{tableId}/records
```

```bash
curl -X POST \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "fields": {
      "Name": "New Customer",
      "Status": "Active",
      "Email": "new@example.com"
    }
  }' \
  "https://api.ninox.com/v1/teams/TEAM_ID/databases/DB_ID/tables/TABLE_ID/records"
```

#### Update Record
```
PUT /v1/teams/{teamId}/databases/{databaseId}/tables/{tableId}/records/{recordId}
```

```bash
curl -X PUT \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"fields": {"Status": "Inactive"}}' \
  "https://api.ninox.com/v1/teams/TEAM_ID/databases/DB_ID/tables/TABLE_ID/records/1"
```

#### Delete Record
```
DELETE /v1/teams/{teamId}/databases/{databaseId}/tables/{tableId}/records/{recordId}
```

---

### Execute Ninox Scripts via API

You can run any Ninox NX script via the REST API using the `/exec` endpoint:

```
POST /v1/teams/{teamId}/databases/{databaseId}/exec
```

```bash
curl -X POST \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"query": "users()"}' \
  "https://api.ninox.com/v1/teams/TEAM_ID/databases/DB_ID/exec"
```

```bash
"Example: Create a record via script"
curl -X POST \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"query": "let r := create Customers; r.(Name := \"Test\"); number(r)"}' \
  "https://api.ninox.com/v1/teams/TEAM_ID/databases/DB_ID/exec"
```

### Read-Only Query

```
GET /v1/teams/{teamId}/databases/{databaseId}/{query}
POST /v1/teams/{teamId}/databases/{databaseId}/query
```

### Database Changes (Track Changes)

```
GET /v1/teams/{teamId}/databases/{databaseId}/changes?sinceSq={sequence}
GET /v1/teams/{teamId}/databases/{databaseId}/tables/{tableId}/changes?sinceSq={sequence}
GET /v1/teams/{teamId}/databases/{databaseId}/tables/{tableId}/records/{recordId}/changes?sinceSq={sequence}
```

Use `sinceSq` to poll for changes since a specific sequence number.

### File Management

```
GET    /v1/teams/{teamId}/databases/{databaseId}/tables/{tableId}/records/{recordId}/files
GET    /v1/teams/{teamId}/databases/{databaseId}/tables/{tableId}/records/{recordId}/files/{file}
POST   /v1/teams/{teamId}/databases/{databaseId}/tables/{tableId}/records/{recordId}/files
DELETE /v1/teams/{teamId}/databases/{databaseId}/tables/{tableId}/records/{recordId}/files/{file}
GET    /v1/teams/{teamId}/databases/{databaseId}/tables/{tableId}/records/{recordId}/files/{file}/metadata
GET    /v1/teams/{teamId}/databases/{databaseId}/tables/{tableId}/records/{recordId}/files/{file}/thumb.jpg
```

### Delete Multiple Records

```
DELETE /v1/teams/{teamId}/databases/{databaseId}/tables/{tableId}/records
```

### Search / Lookup Record

```
POST /v1/teams/{teamId}/databases/{databaseId}/tables/{tableId}/record
```

---

## Private Cloud / On-Premises API

Private Cloud and On-Premises deployments use the same endpoint structure as the Public Cloud, but with a different base URL.

### Private Cloud Base URL

```
https://{yoursubdomain}.ninoxdb.de/v1/
```

Or for On-Premises with a custom domain:

```
https://ninox.yourdomain.com/v1/
```

### Additional Endpoints (Private Cloud Only)

Private Cloud adds view management endpoints not available in the Public Cloud API:

```
GET    /v1/teams/{teamId}/databases/{databaseId}/views
GET    /v1/teams/{teamId}/databases/{databaseId}/tables/{tableId}/views
GET    /v1/teams/{teamId}/databases/{databaseId}/tables/{tableId}/views/{viewId}/share
POST   /v1/teams/{teamId}/databases/{databaseId}/tables/{tableId}/views/{viewId}/share
DELETE /v1/teams/{teamId}/databases/{databaseId}/tables/{tableId}/views/{viewId}/share
```

Private Cloud also supports file sharing links:

```
GET    /v1/teams/{teamId}/databases/{databaseId}/tables/{tableId}/records/{recordId}/files/{file}/share
POST   /v1/teams/{teamId}/databases/{databaseId}/tables/{tableId}/records/{recordId}/files/{file}/share
DELETE /v1/teams/{teamId}/databases/{databaseId}/tables/{tableId}/records/{recordId}/files/{file}/share
```

### Authentication — Private Cloud Specifics

```
Authorization: Bearer {accessToken}
Content-Type: application/json
```

Note: On Private Cloud, even requests with an empty body **may require** a body (`{}`) to be sent. This is a quirk noted in the official docs: "A body is required when accessing Ninox data."

### Upsert (Create or Update)

Both Public and Private Cloud support upsert via a special field in the POST body:

```bash
POST /v1/teams/{teamId}/databases/{databaseId}/tables/{tableId}/records

{
  "_upsert": ["MatchField"],
  "fields": {
    "MatchField": "unique-value",
    "OtherField": "data"
  }
}
```

This creates a record if no match is found, or updates it if a record with the matching field value exists.

---

## Rate Limits

### Monthly API Call Limits (by Plan)

| Plan | Monthly API Calls |
|---|---|
| Starter | 1,500 |
| Professional | 30,000 |
| Enterprise | Unlimited |

**Note**: Usage is updated every 5 minutes in the Ninox dashboard. You can monitor consumption under Settings → Integrations → API Usage.

### Per-Minute Limits

[UNVERIFIED] — Ninox does not publish per-minute rate limits in official documentation. Community reports suggest no hard per-second throttling at low volumes, but this is unconfirmed. If you encounter HTTP 429 responses, implement exponential backoff.

### Best Practices to Avoid Rate Limits

- Use the `/exec` endpoint to batch multiple operations in a single API call instead of individual record endpoints
- Use `select` with `where` filters in exec queries to reduce data transferred
- Implement caching of infrequently changing data

---

## HTTP Status Codes

| Code | Meaning |
|---|---|
| 200 | OK |
| 201 | Created |
| 202 | Accepted |
| 204 | No Content |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 422 | Unprocessable Entity |
| 500 | Internal Server Error |

---

## Data Format Notes

### Field Values in API
| Ninox Field Type | API Format |
|---|---|
| text | `"string value"` |
| number | `42` or `3.14` |
| boolean | `true` or `false` |
| date | `"2024-03-15"` (ISO 8601) |
| datetime | `"2024-03-15T14:30:00.000Z"` |
| choice | `"ChoiceOptionName"` |
| reference | `42` (the numeric ID of the related record) |
| multiplechoice | `["Option1", "Option2"]` |

---

## Integration with External Tools

### Zapier
Ninox can be integrated with Zapier via the Ninox Zapier app or directly via REST API with HTTP actions.

### Make (formerly Integromat)
Integration via REST API possible using HTTP module with Bearer token auth.

### n8n
Use the HTTP Request node with Bearer token authentication.

---

## Important Notes

1. **Authentication**: Always use correct API key — store it securely, never in NX scripts visible to all users
2. **Error Handling**: HTTP requests can fail — implement conditional checks
3. **Rate Limits**: Monthly limits depend on plan (see Rate Limits section above); per-minute limits are unverified
4. **API versioning**: Endpoints above are for v1 — verify against your Ninox version
5. **Private Cloud URL**: Ask your Ninox admin for the exact base URL — it varies per deployment

---

**Sources**:
- Official REST API Docs: https://docs.ninox.com/en/api
- Introduction to Ninox API: https://forum.ninox.com/t/83yzlg7/introduction-to-ninox-api
- Public Cloud Endpoints: https://forum.ninox.com/t/35yzp89/api-endpoints-for-public-cloud
- Private Cloud Endpoints: https://forum.ninox.com/t/x2yzfmf/api-endpoints-for-private-cloudon-premises
- Rate Limits / Capacity: https://forum.ninox.com/t/p8yzfcy/understand-usage-overview-and-capacity-restrictions
- HTTP Script Docs: `references/http-requests.md`
- Database Schema: `references/database-schema.md`
