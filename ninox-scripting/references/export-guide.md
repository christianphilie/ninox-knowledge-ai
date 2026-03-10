# How to Export Your Ninox Database Structure for AI Analysis

## Overview

When you want Claude to help you write Ninox scripts, the AI needs to know your exact database structure. Field names, table names, and relations in Ninox are project-specific — there is no universal standard. This guide explains how to get the right information.

---

## Option A — REST API (Recommended for Developers)

### Step 1: Get Your API Key

1. Open Ninox
2. Go to **Settings** (gear icon)
3. Navigate to **Integrations** or **API**
4. Find or generate your **API Key** (Personal Access Token)
5. Copy the key — keep it secret!

### Step 2: Find Your Team ID

```bash
curl -H "Authorization: Bearer YOUR_API_KEY" \
  https://api.ninox.com/v1/teams
```

Response example:
```json
[{"id": "abc123", "name": "My Company"}]
```

### Step 3: Find Your Database ID

```bash
curl -H "Authorization: Bearer YOUR_API_KEY" \
  https://api.ninox.com/v1/teams/TEAM_ID/databases
```

Response example:
```json
[
  {"id": "db001", "name": "CRM"},
  {"id": "db002", "name": "Inventory"}
]
```

### Step 4: List All Tables

```bash
curl -H "Authorization: Bearer YOUR_API_KEY" \
  https://api.ninox.com/v1/teams/TEAM_ID/databases/DB_ID/tables
```

Response example:
```json
[
  {"id": "tbl_a", "name": "Customers"},
  {"id": "tbl_b", "name": "Orders"},
  {"id": "tbl_c", "name": "Order Positions"}
]
```

### Step 5: Get Fields for Each Table

```bash
# Get fields for Customers table
curl -H "Authorization: Bearer YOUR_API_KEY" \
  "https://api.ninox.com/v1/teams/TEAM_ID/databases/DB_ID/tables/tbl_a/fields"
```

Response example:
```json
[
  {"id": "A", "name": "Name", "type": "text"},
  {"id": "B", "name": "Email", "type": "email"},
  {"id": "C", "name": "Status", "type": "choice", "choices": ["Active", "Inactive"]},
  {"id": "D", "name": "Orders", "type": "backreference", "referenceTable": "tbl_b"}
]
```

### Complete Shell Script for Full Export

```bash
#!/bin/bash
API_KEY="YOUR_API_KEY"
TEAM_ID="YOUR_TEAM_ID"
DB_ID="YOUR_DB_ID"
BASE="https://api.ninox.com/v1"

echo "=== Ninox Database Schema Export ==="
echo ""

# Get tables
TABLES=$(curl -s -H "Authorization: Bearer $API_KEY" "$BASE/teams/$TEAM_ID/databases/$DB_ID/tables")
echo "Tables: $TABLES"
echo ""

# Get fields for each table (requires jq)
echo "$TABLES" | jq -r '.[].id' | while read TABLE_ID; do
  TABLE_NAME=$(echo "$TABLES" | jq -r ".[] | select(.id == \"$TABLE_ID\") | .name")
  echo "=== Table: $TABLE_NAME (ID: $TABLE_ID) ==="
  curl -s -H "Authorization: Bearer $API_KEY" \
    "$BASE/teams/$TEAM_ID/databases/$DB_ID/tables/$TABLE_ID/fields" | jq .
  echo ""
done
```

---

## Option B — Manual Export from the UI

The Ninox UI does not provide a dedicated "schema export" feature. However, you can manually gather the information:

### From the Table Editor
1. Open the database in Ninox
2. Click on a table
3. Click the **Edit** (pencil) icon on a table
4. You can see all fields with their types
5. Screenshot or manually list: field name, type, whether it's a relation

### From Formula Fields
1. Click on a formula field
2. The formula script is visible in the editor
3. Copy-paste relevant formulas for context

### CSV/Excel Export (Data Only)
Ninox supports exporting table data as CSV or Excel:
- This gives you data but NOT the schema
- Useful for showing example data (3-5 rows per table)
- Does not show relation structure

### Backup Files
Ninox allows exporting database backups (`.ninox` files):
- Contains the full database including schema
- Binary format — not directly readable
- Useful for backup but not for AI analysis

---

## What the AI Needs

For effective AI-assisted scripting, provide:

### Essential (Always Needed)
1. **Table names** — exact spelling, including spaces and special characters (use quotes for multi-word names: `'Order Positions'`)
2. **Field names per table** — exact spelling + type
3. **Relations** — which tables reference each other and via which fields

### Very Helpful
4. **Existing script examples** — shows patterns already used in your database
5. **The trigger context** — where will the script run? (onChange on field X, Button on table Y, etc.)

### Optional but Useful
6. **Sample data** — 3-5 rows per table to understand data formats
7. **Business logic description** — what the script should achieve

---

## Prompt Template for Claude

Copy and fill in this template when asking Claude for Ninox scripting help:

```
I'm working with a Ninox database and need help writing a script.

## Database Structure

### Table: [Table Name 1]
Fields:
- [FieldName]: [type] — [description if needed]
- [FieldName]: [type]
- [RelationField]: relation to [Other Table]

### Table: [Table Name 2]
Fields:
- [FieldName]: [type]
- [FieldName]: [type]
- [RelationField]: relation to [Table Name 1] (back-reference, many orders per customer)

## Relations
- [Table A].[FieldName] → [Table B] (N:1: many orders per customer)
- [Table B].[RelationField] ← [Table A] (1:N back-reference)

## Trigger Context
This script runs as: [onChange on field "Status" in table "Orders"]
OR: [Button trigger in table "Orders"]

## Existing Scripts (for context)
[Paste any existing relevant formula or trigger scripts]

## What I Need
[Describe what the script should do]
```

### Concrete Example

```
I'm working with a Ninox database and need help writing a script.

## Database Structure

### Table: Customers
Fields:
- Name: text
- Email: text
- Status: choice (Active, Inactive, Prospect)
- 'Customer Since': date
- Orders: back-relation to Orders table

### Table: Orders
Fields:
- 'Order Number': autonumber
- Customer: relation to Customers (N:1)
- Date: date
- Status: choice (New, Processing, Shipped, Completed, Cancelled)
- Total: formula (sum of positions)
- 'Order Positions': back-relation to 'Order Positions' table

### Table: Order Positions
Fields:
- Order: relation to Orders (N:1)
- Article: text
- Quantity: number
- 'Unit Price': number
- Amount: formula (Quantity * 'Unit Price')

## Relations
- Orders.Customer → Customers (many orders per customer)
- 'Order Positions'.Order → Orders (many positions per order)

## Trigger Context
Button trigger in table "Orders"

## What I Need
When the button is clicked, I want to:
1. Set Status to "Processing"
2. Send a webhook to https://hooks.example.com/order-status with the order data
3. Show a confirmation message to the user
```

---

## Quick Reference: Common Field Name Patterns

Ninox users commonly name fields in various ways. If you're guessing field names (which you should avoid), these are common patterns:

| Concept | Common Names |
|---|---|
| Primary name | `Name`, `Title`, `Bezeichnung` |
| Status | `Status`, `Zustand`, `State` |
| Date created | `'Created At'`, `Erstellungsdatum`, `Datum` |
| Date modified | `'Last Modified'`, `'Geändert am'` |
| Position number | `Pos`, `'Pos Nr'`, `Position`, `Lfd. Nr.` |
| Amount | `Amount`, `Betrag`, `Summe` |
| Quantity | `Quantity`, `Menge`, `Anzahl` |
| Unit | `Unit`, `Einheit`, `Mengeneinheit` |

**Important**: Never guess — always ask or check the actual field names!

---

## Sources

- Ninox REST API: https://docs.ninox.com/en/api
- Database Schema: `references/database-schema.md`
- Context Queries: `references/context-queries.md`
