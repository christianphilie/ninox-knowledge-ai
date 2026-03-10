# Ninox Trigger System

## Overview

Triggers are scripts that execute automatically in response to specific events in Ninox. They run in the context of a specific record (`this`).

**Key verified facts from official docs**:
- Triggers execute **server-side** (equivalent to running inside `do as server`)
- `onCreate` and `onChange` (field trigger) will **NOT fire if triggered by a script** — only from user interaction
- `onCreate` will **NOT execute during data import**
- Field-level triggers fire **before** table-level triggers
- UI functions (`alert()`, `dialog()`, `openTable()`, `popupRecord()`) are **NOT available** in triggers — only in buttons or "Trigger after open" in database options

**Source**: Ninox Documentation, https://docs.ninox.com/en/script/triggers

---

## Trigger Types

### 1. onChange (Field Trigger)
**When**: Fires when a specific field value changes.
**Context**: The record containing the field (`this`).
**Access**: `this` (current record), field values, relations.

```ninox
"Trigger: onChange on field 'Status'"
"Script runs whenever Status field changes"
let my := this;
if my.Status = "Completed" then
    my.'Completion Date' := today();
end
```

**Important Notes**:
- Fires for each individual field that has a trigger
- Does NOT fire when the record is created for the first time (use onCreate for that)
- Can access the new value via `this.FieldName`
- Cannot reliably access the previous value (no built-in "old value" concept)

---

### 2. onCreate (Record Trigger)
**When**: Fires when a new record is created.
**Context**: The newly created record (`this`).

```ninox
"Trigger: onCreate"
"Script runs when a new record is created"
let my := this;
my.'Created By' := user();
my.'Created At' := now();
my.Status := "New";
```

**Important Notes**:
- Fires once when the record is first saved
- Good for setting default values and audit fields
- `user()` returns the current user

---

### 3. onSave (Record Trigger)
**When**: Fires every time a record is saved (create or update).
**Context**: The saved record (`this`).

```ninox
"Trigger: onSave"
"Script runs on every save"
let my := this;
my.'Last Modified' := now();
my.'Modified By' := user();
```

**Important Notes**:
- Fires on both create AND update
- Can cause performance issues if heavy computation is done here
- Runs synchronously before the save completes

---

### 4. onDelete (Record Trigger)
**When**: Fires before a record is deleted.
**Context**: The record about to be deleted (`this`).

```ninox
"Trigger: onDelete"
"Script runs before record deletion"
let my := this;
"Log deletion"
let logEntry := create AuditLog;
logEntry.Action := "DELETE";
logEntry.RecordID := text(number(my));
logEntry.DeletedAt := now();
logEntry.DeletedBy := user();
```

**Important Notes**:
- Fires BEFORE deletion
- Can be used for cleanup of related records
- Cannot cancel the deletion from within the trigger

---

### 5. Database Trigger (Trigger after Open)
**When**: Fires when the database is opened.
**Context**: No specific record context.

```ninox
"Trigger: Database 'after open'"
"Runs when users open this database"
"Can be used for initialization, showing welcome messages"
alert("Welcome to the CRM! Today is " + format(today(), "DD.MM.YYYY"))
```

**Important Notes**:
- Available in Database Options (not on individual tables)
- UI functions like `alert()` ARE available here (unlike table/field triggers)
- Runs client-side, can show dialogs

---

### 6. Button Trigger
**When**: Fires when a user clicks a button field.
**Context**: The record on which the button is placed (`this`).

```ninox
"Trigger: Button click"
"Manual execution by user"
let my := this;
do as transaction
    my.Status := "Processed";
    my.'Processed By' := user();
    my.'Processed At' := now();
end;
alert("Record has been processed!")
```

**Important Notes**:
- Only runs when the user explicitly clicks
- Has full access to `this`, UI functions (`alert()`, `openRecord()`, `popupRecord()`)
- Can trigger HTTP requests (unlike some other triggers)
- Can navigate to other records

---

### 6. Formula Fields (Computed Fields)
**When**: Recalculated whenever any dependency changes.
**Context**: The current record (`this`).
**Return**: The computed value (must match the field type).

```ninox
"Formula field: TotalAmount"
"Returns calculated value"
let my := this;
sum(my.Positions[Amount * Quantity])
```

**Important Notes**:
- Must return a value of the correct type
- Should be side-effect free (no write operations)
- Cannot perform HTTP requests
- Recalculated lazily or when dependencies change

---

## Context Access in Triggers

### `this` — Current Record
All triggers have access to `this`, which is the record the trigger runs on.

**Best Practice**: Always store in a variable at the start:
```ninox
let my := this;
```

### `user()` — Current User
Returns information about the logged-in user.

```ninox
"⚠️ Not fully documented, but works"
let currentUser := user();
"user() returns a user record/object"
"Access user fields like:"
let userName := user().Name;    "Name of the user"
let userEmail := user().Email;  "Email of the user (if available)"
```

**Note**: The exact fields available on `user()` depend on your Ninox configuration.

### `now()` and `today()`
Always available, return current timestamp/date:
```ninox
let currentTime := now();   "datetime"
let currentDate := today(); "date"
```

---

## HTTP Requests in Triggers

HTTP requests via `http()` are **only available in certain contexts**:

| Trigger Type | HTTP Requests |
|---|---|
| Button (onClick) | ✅ Yes — use `do as server` block |
| onChange | ⚠️ Limited — use `do as server` block |
| onCreate | ⚠️ Limited — use `do as server` block |
| onSave | ⚠️ Limited — use `do as server` block |
| onDelete | ⚠️ Limited |
| Formula field | ❌ No — formula fields must be pure |

**Pattern for HTTP in triggers**:
```ninox
"Always wrap HTTP calls in 'do as server'"
do as server
    let response := http("https://api.example.com/webhook", {
        method: "POST",
        headers: {
            "Content-Type": "application/json",
            "Authorization": "Bearer myToken"
        },
        body: {
            "recordId": text(number(this)),
            "status": this.Status
        }
    });
end
```

---

## Common Trigger Mistakes

### Mistake 1: Infinite Loop in onChange
```ninox
"❌ WRONG: onChange on field 'Total' that writes to 'Total'"
"This creates an infinite loop!"
this.Total := this.Amount * this.Quantity;
```

```ninox
"✅ CORRECT: Write to a different field"
"OR: Use a formula field for computed values instead"
this.CachedTotal := this.Amount * this.Quantity;
```

### Mistake 2: Heavy Computation in onSave
```ninox
"❌ WRONG: Expensive operation on every save"
"onSave trigger"
let allRelated := select BigTable where RelatedField = this;
for r in allRelated do
    "Heavy processing";
end
```

```ninox
"✅ BETTER: Use a button for expensive operations"
"Or use 'do as deferred' to avoid blocking"
do as deferred
    let allRelated := select BigTable where RelatedField = this;
    for r in allRelated do
        "Processing";
    end
end
```

### Mistake 3: Not Using `let my := this` in Loops
```ninox
"❌ PROBLEMATIC: this can lose context in loops"
"onSave trigger"
for pos in 'Positions' do
    pos.Total := pos.Amount * pos.Quantity;
    "Here, this.SomeField may not work reliably"
    pos.Tax := pos.Amount * this.TaxRate;  "RISKY"
end
```

```ninox
"✅ CORRECT: Store this in variable first"
let my := this;
for pos in 'Positions' do
    pos.Total := pos.Amount * pos.Quantity;
    pos.Tax := pos.Amount * my.TaxRate;  "SAFE"
end
```

---

## Execution Order

When multiple triggers fire:
1. onChange triggers fire first (for individual fields)
2. onSave fires after all field changes
3. Formula fields recalculate as needed

---

## Available Functions in Triggers

| Function | onChange (field) | onCreate | onSave | onDelete | DB after open | Button |
|---|---|---|---|---|---|---|
| `this` | ✅ | ✅ | ✅ | ✅ | ❌ | ✅ |
| `user()` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| `select` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| `create` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| `delete` | ✅ | ✅ | ✅ | ⚠️ | ✅ | ✅ |
| `http()` | ✅ (server-side) | ✅ (server-side) | ✅ (server-side) | ✅ (server-side) | ✅ | ✅ |
| `do as server` | ✅ (redundant) | ✅ (redundant) | ✅ (redundant) | ✅ (redundant) | ✅ | ✅ |
| `alert()` | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ |
| `dialog()` | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ |
| `openRecord()` | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ |
| `openTable()` | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ |
| `popupRecord()` | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ |
| `sendEmail()` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |

**Key facts**:
- All table/field triggers run **server-side** automatically — `do as server` is redundant but harmless
- `http()` works in all server-side triggers — no need to add `do as server` wrapping
- UI functions (`alert`, `dialog`, `openRecord`, `popupRecord`) are **only available in Button** triggers and the "Database after open" trigger
- Triggers do **not** fire recursively (script-triggered changes don't re-fire onChange/onCreate)

---

## Sources

- Official Documentation: https://docs.ninox.com/en/script/triggers
- Community Forum: https://forum.ninox.de
- Undocumented Features: `references/undocumented-features.md`

**Note**: Some details (especially which triggers support HTTP) are based on community experience and may not be fully documented. Mark any uncertainty with [UNVERIFIED] in your code.
