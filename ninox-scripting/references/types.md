# Ninox Type System

## Overview

Ninox NX scripting has a static type system with automatic type inference. Unlike JavaScript, types are meaningful — comparing a date with a string or a number with text will not work as expected.

**Source**: Ninox Documentation, https://docs.ninox.com/en/script/variables-and-types

---

## Primitive Types

| Type | Description | Example Value |
|---|---|---|
| `number` | Integer or float | `42`, `3.14`, `-7` |
| `text` | String | `"Hello"`, `""` |
| `boolean` | True/False | `true`, `false` |
| `date` | Calendar date (no time) | `date(2024,3,15)`, `today()` |
| `time` | Time of day | `time(14,30,0)` |
| `datetime` | Date + time | `now()`, `datetime(d, t)` |
| `color` | Color value | Used in color-coded fields |

### Record/Table Types
A record from a table has its table as its type:
```ninox
let rec := first(select Customers);
"rec has type 'Customers' (the table name)"
```

### Collection Types
A collection from `select` or a relation is an ordered set of records. Collections do NOT have the same methods as JavaScript arrays.

---

## Type Conversion Functions

### text(value) → text
Converts any value to text.
```ninox
text(42)          "→ '42'"
text(3.14)        "→ '3.14'"
text(true)        "→ 'true'"
text(today())     "→ date formatted as text (locale-dependent)"
text(number(rec)) "→ ID of record as text"
```

### number(value) → number
Converts a value to a number.
```ninox
number("42")      "→ 42"
number("3.14")    "→ 3.14"
number(true)      "→ 1"
number(false)     "→ 0"
number(record)    "→ internal ID/No of the record (⚠️ undocumented but works)"
number("")        "→ 0 (empty string → 0)"
number(null)      "→ 0"
```

### boolean(value) → boolean
Converts to boolean.
```ninox
boolean(1)        "→ true"
boolean(0)        "→ false"
boolean("")       "→ false (empty string is falsy)"
boolean("text")   "→ true (non-empty string is truthy)"
boolean(null)     "→ false"
```

---

## Implicit Type Coercion

Ninox performs some implicit coercion, but it is NOT JavaScript — many things that work in JS will fail in Ninox.

### String Concatenation
Use `+` operator or `concat()` for text concatenation:
```ninox
"Using + (works with text)"
"Hello " + "World"           "→ 'Hello World'"
"Value: " + text(42)         "→ 'Value: 42'"

"Using concat()"
concat("Hello ", "World")    "→ 'Hello World'"
concat("Value: ", text(42))  "→ 'Value: 42'"
```

**Important**: `+` with mixed types requires explicit conversion:
```ninox
"❌ WRONG: Mixing types with +"
"Hello " + 42                "→ May not work as expected"

"✅ CORRECT: Explicit conversion"
"Hello " + text(42)          "→ 'Hello 42'"
```

### Number Operations
Standard arithmetic operators work on numbers:
```ninox
5 + 3      "→ 8"
5 - 3      "→ 2"
5 * 3      "→ 15"
5 / 3      "→ 1.666..."
5 mod 3    "→ 2 (modulo)"
```

**Note**: `mod` is the modulo operator in Ninox (not `%`).

---

## Null / Empty Value Handling

### What is null in Ninox?
- Empty text fields return `""` (empty string)
- Empty number fields return `0` or `null` depending on context
- Empty date fields return `null`
- Unset relation fields return `null`

### Truthiness
In `if` conditions:
| Value | Truthy? |
|---|---|
| `0` | false |
| `""` (empty string) | false |
| `null` | false |
| `false` | false |
| Any non-zero number | true |
| Any non-empty string | true |
| Any record reference | true |
| Any date | true |

```ninox
"Check if a field has a value"
if this.Name then
    "Name is not empty";
end

if this.Amount then
    "Amount is not zero";
end

if this.'Related Record' then
    "Relation is set";
end
```

### Null Checks
```ninox
"Explicit null check"
if this.DateField = null then
    "Date is not set";
end

if this.TextField = "" then
    "Text is empty";
end

if this.NumberField = 0 then
    "Number is zero (or unset)";
end

"Safe access pattern"
let val := if this.Amount then this.Amount else 0 end;
```

### Null in Arithmetic
```ninox
"null in arithmetic returns null/0"
let n := null + 5;    "→ 5 (null treated as 0)"
let n2 := null * 5;   "→ 0"
```

---

## Type Checking

Ninox does NOT have a built-in `typeof` or `is` operator for type checking. Use these workarounds:

### Check if value is numeric
```ninox
"⚠️ Workaround"
let isNum := number(text(value)) = value;
"This checks if converting to text and back gives the same number"
```

### Check if value is empty/null
```ninox
"Works for all types"
if not value then
    "Value is empty, null, zero, or false";
end
```

### Check if collection is empty
```ninox
let items := select Table where Condition;
if count(items) = 0 then
    "No records found";
end

"Or using not:"
if not items then
    "⚠️ May work for empty collections";
end
```

---

## Data Types for Field Definitions

When defining custom function parameters, use these type names:

```ninox
function myFunc(name: text, count: number, active: boolean, birthDate: date): text do
    "Function body"
    "Returns text"
end
```

**Supported parameter types**:
- `text`
- `number`
- `boolean`
- `date`
- `time`
- `datetime`
- Table type names (e.g., `'Customers'`) — pass a record

---

## Common Type Errors

### Error: Wrong type in comparison
```ninox
"❌ WRONG: Comparing date with string"
if this.Deadline < "2024-12-31" then  "Error or wrong result"
    "...";
end

"✅ CORRECT"
if this.Deadline < date(2024, 12, 31) then
    "...";
end
```

### Error: Using result of format() as date
```ninox
"❌ WRONG"
let formatted := format(today(), "YYYY-MM-DD");
"formatted is TEXT, not a date!"
if formatted < '2024-12-31' then  "String comparison, not date comparison"
    "...";
end

"✅ CORRECT: Keep it as date"
if today() < date(2024, 12, 31) then
    "...";
end
```

### Error: concat() with non-text arguments
```ninox
"❌ WRONG"
concat(this.FirstName, " ", this.Age)  "Age is a number — may cause error"

"✅ CORRECT"
concat(this.FirstName, " ", text(this.Age))
```

---

## Sources

- Official Documentation: https://docs.ninox.com/en/script/variables-and-types
- Community Forum: https://forum.ninox.de

**Note**: Some null-handling behavior is based on community experience and practical testing. The exact behavior may vary by Ninox version.
