# Ninox Database Schema Structure

## Overview

Understanding the Ninox database schema is helpful when analyzing an existing database, building integrations, or using the REST API. This document describes how Ninox databases are structured internally and via the API.

**Source**: Ninox API Documentation, Community Forum, REST API exploration.

---

## Conceptual Structure

```
Ninox Account
└── Team / Workspace (has an ID)
    └── Database (has an ID, name)
        └── Table (has an ID, name)
            ├── Fields (id, name, type, options)
            └── Records (rows of data)
```

---

## Field Types in Ninox

Ninox supports the following field types. These names appear in the schema/API responses:

### Basic Types
| Field Type Name | Description |
|---|---|
| `text` | Single-line text |
| `string` | Multi-line text / notes |
| `number` | Numeric value |
| `boolean` | Checkbox (yes/no) |
| `date` | Date only |
| `time` | Time only |
| `datetime` | Date + time |
| `daterange` | Date range (start + end) |
| `datetimerange` | Date/time range (appointments) |

### Selection Types
| Field Type Name | Description |
|---|---|
| `choice` | Single selection (dropdown) |
| `multiplechoice` | Multiple selection (tags) |
| `dynamicchoice` | Dynamic selection from another table |

### Reference / Relation Types
| Field Type Name | Description |
|---|---|
| `reference` | N:1 relation — points to one record in another table |
| `backreference` | 1:N back-relation — all records in another table pointing to this |

### File / Media Types
| Field Type Name | Description |
|---|---|
| `file` | File attachment |
| `image` | Image field |
| `signature` | Signature field |

### Computed / Scripted Types
| Field Type Name | Description |
|---|---|
| `formula` | Computed field (formula/script that returns a value) |
| `trigger` | Button or event trigger (contains a script) |
| `view` | Embedded view of related records |

### Special Types
| Field Type Name | Description |
|---|---|
| `autonumber` | Auto-incrementing number |
| `color` | Color selector |
| `html` | HTML content field |
| `url` | URL field |
| `email` | Email field |
| `phone` | Phone number field |
| `location` | Geographic location |
| `rating` | Star rating |
| `progress` | Progress bar (0-100) |

**Note**: The exact type names in the API may vary by Ninox version. Use the REST API to inspect actual type names in your database. [UNVERIFIED — cross-reference with your API response]

---

## REST API Schema Response

When you query the API for table fields, the response looks like this:

### Table Fields Endpoint
```
GET https://api.ninox.com/v1/teams/{teamId}/databases/{databaseId}/tables/{tableId}/fields
```

### Example Response Structure
```json
[
  {
    "id": "A",
    "name": "Name",
    "type": "text"
  },
  {
    "id": "B",
    "name": "Status",
    "type": "choice",
    "choices": ["New", "In Progress", "Done"]
  },
  {
    "id": "C",
    "name": "Amount",
    "type": "number"
  },
  {
    "id": "D",
    "name": "Customer",
    "type": "reference",
    "referenceTable": "customers_table_id"
  },
  {
    "id": "E",
    "name": "Positions",
    "type": "backreference",
    "referenceTable": "positions_table_id",
    "referenceField": "F"
  },
  {
    "id": "F",
    "name": "Total",
    "type": "formula",
    "formula": "sum(Positions[Amount])"
  }
]
```

### Field ID System
Ninox uses short letter IDs (A, B, C, ..., Z, AA, AB, ...) for fields internally. The human-readable names are separate. When the API returns record data, it may use either field IDs or field names depending on the endpoint.

---

## Record Data Structure

### Reading Records
```
GET https://api.ninox.com/v1/teams/{teamId}/databases/{databaseId}/tables/{tableId}/records
```

### Example Record Response
```json
{
  "id": 1,
  "sequence": 1,
  "createdAt": "2024-03-15T10:30:00.000Z",
  "updatedAt": "2024-03-15T14:00:00.000Z",
  "fields": {
    "Name": "Acme Corp",
    "Status": "Active",
    "Amount": 5000,
    "Customer": 42,
    "CreatedDate": "2024-03-15"
  }
}
```

**Notes**:
- `id` is Ninox's internal record identifier
- Relations reference the numeric `id` of the related record
- Dates are returned as ISO strings: `"YYYY-MM-DD"`
- Datetimes: `"YYYY-MM-DDTHH:mm:ss.sssZ"`

---

## Table Structure (Database Level)

### List Tables Endpoint
```
GET https://api.ninox.com/v1/teams/{teamId}/databases/{databaseId}/tables
```

### Example Response
```json
[
  {
    "id": "table_abc123",
    "name": "Customers"
  },
  {
    "id": "table_def456",
    "name": "Orders"
  },
  {
    "id": "table_ghi789",
    "name": "Order Positions"
  }
]
```

---

## How Relations Are Modeled

### N:1 Relation (reference field)
Table `Orders` has a field `Customer` of type `reference` pointing to table `Customers`.

In NX Script:
```ninox
"Reading the relation"
let customer := this.Customer;  "Returns a Customers record"
let custName := this.Customer.Name;  "Access field on related record"
```

In the API:
```json
{
  "Customer": 42  "The ID number of the related customer record"
}
```

### 1:N Relation (backreference)
Table `Customers` has a backreference field `Orders` pointing back to all orders for this customer.

In NX Script:
```ninox
"Iterating over backreference"
for order in this.Orders do
    "Process each order";
end
```

In the API, backreferences are typically NOT included in the record response by default — you need to query the related table with a filter.

---

## How Formulas / Scripts Are Stored

Formula fields contain the NX script as a text string. In the API or export:

```json
{
  "id": "F",
  "name": "Total",
  "type": "formula",
  "formula": "sum('Order Positions'[Amount * Quantity])"
}
```

Trigger scripts (buttons, onChange, etc.) are stored similarly but under a different property depending on the trigger type.

**Note**: The exact schema format for scripts/formulas is [UNVERIFIED] — it depends on the Ninox version and API used. When exporting for AI analysis, copy-paste the formula text manually if needed.

---

## Exporting Schema for AI Analysis

The most useful information for AI-assisted scripting:

1. **Table names** (exact spelling, including spaces and special characters)
2. **Field names per table** (exact spelling + type)
3. **Relation structure** (which table points to which, field name)
4. **Existing formula examples** (helps AI understand your patterns)

See `references/export-guide.md` for step-by-step instructions.

---

## Sources

- Ninox REST API: https://docs.ninox.com/en/api/endpoints
- Community Forum: https://forum.ninox.de
- REST API exploration (field types may vary by Ninox version)

**Note**: Schema details marked [UNVERIFIED] should be confirmed against your specific Ninox version and API response.
