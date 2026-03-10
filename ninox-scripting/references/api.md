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
| On-Premises | Check your Ninox admin for URL |

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
```

Use `sinceSq` to poll for changes since a specific sequence number.

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
3. **Rate Limits**: Rate limits are [UNVERIFIED] — may depend on your Ninox plan
4. **API versioning**: Endpoints above are for v1 — verify against your Ninox version

---

**Sources**:
- Official REST API Docs: https://docs.ninox.com/en/api
- Authentication: https://docs.ninox.com/en/api/authentication
- Endpoints: https://docs.ninox.com/en/api/endpoints
- HTTP Script Docs: `references/http-requests.md`
- Database Schema: `references/database-schema.md`
