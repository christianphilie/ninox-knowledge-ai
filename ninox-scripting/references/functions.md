# Ninox Scripting - Functions

## Overview

This documentation describes the documented functions in Ninox scripting.

**Source**: https://forum.ninox.de/category/docs

---

## Basic Syntax

### Variables
```ninox
let variable := value;
variable := newValue;
```

**Source**: https://ninox.com/de/tutorials/basic-scripting

### Conditional Logic
```ninox
if condition then
  "Code";
else
  "Code";
end
```

**Source**: https://ninox.com/de/tutorials/if-then-else-statement

### Loops
```ninox
while condition do
  "Code";
end

for item in collection do
  "Code";
end
```

**Source**: Ninox Tutorials

---

## Database Operations

### Select Statements

#### Basic Select
```ninox
select Table
```

#### Select with Where
```ninox
select Table where condition
```

**Performance Note**: `where` filters during selection (server-side), therefore prefer to use.

**Source**: https://ninox.com/de/tutorials/advanced-tutorial-2

#### Select with Order By
```ninox
select Table order by field
select Table order by field desc
```

#### Select with Limit
```ninox
select Table limit 10
```

---

## Aggregate Functions

### Mathematical Functions
- `sum(collection)` - Sum of all values
- `count(collection)` - Number of records
- `average(collection)` - Average value
- `min(collection)` - Minimum
- `max(collection)` - Maximum

**Example**:
```ninox
let total := sum(select Orders[Amount]);
let count := count(select Orders);
```

**Source**: Ninox Documentation - Functions

### Record Functions
- `first(collection)` - First record
- `last(collection)` - Last record

**Example**:
```ninox
let firstOrder := first(select Orders order by Date);
```

---

## String Functions

- `concat(string1, string2, ...)` - Concatenate strings
- `contains(text, search)` - Checks if text is contained
- `startsWith(text, prefix)` - Checks if text starts with prefix
- `endsWith(text, suffix)` - Checks if text ends with suffix
- `length(text)` - Text length
- `upper(text)` - Convert to uppercase
- `lower(text)` - Convert to lowercase

**Example**:
```ninox
let fullName := concat(firstName, " ", lastName);
if contains(email, "@") then
  "Email is valid";
end
```

**Source**: Ninox Documentation - Functions

---

## Date and Time Functions

### Current Values
- `today()` - Today's date
- `now()` - Current timestamp

### Create Date
- `date(year, month, day)` - Create date
- `time(hour, minute, second)` - Create time
- `datetime(date, time)` - Combine date and time

### Extract Date
- `year(date)` - Year
- `month(date)` - Month
- `day(date)` - Day
- `weekday(date)` - Day of week
- `week(date)` - Calendar week
- `quarter(date)` - Quarter

**Example**:
```ninox
let today := today();
let currentYear := year(today);
let currentQuarter := quarter(today);
```

**Source**: Ninox Documentation - Functions

---

## Mathematical Functions

- `abs(number)` - Absolute value
- `round(number)` - Round
- `floor(number)` - Round down
- `ceil(number)` - Round up
- `sqrt(number)` - Square root
- `pow(base, exponent)` - Power

**Example**:
```ninox
let rounded := round(3.7); "4"
let absolute := abs(-5); "5"
```

**Source**: Ninox Documentation - Functions

---

## Logical Functions

- `and(condition1, condition2)` - Logical AND
- `or(condition1, condition2)` - Logical OR
- `not(condition)` - Logical negation

**Example**:
```ninox
if and(age >= 18, hasLicense) then
  "Can drive car";
end
```

**Source**: Ninox Documentation

---

## Custom Functions

### Define Function
```ninox
function calculateTotal(amount, tax) do
  amount + (amount * tax)
end
```

### Parameter Types
- `text`
- `number`
- `boolean`
- `date`
- `time`
- `datetime`
- Table types

**Source**: https://forum.ninox.com/t/x2yz1t2/create-your-own-functions

---

## Performance Optimizations

### Transaction Blocks
```ninox
do as transaction
  "Multiple write operations";
  record1.field := value1;
  record2.field := value2;
end
```

**Usage**: For multiple write operations for better performance.

**Source**: https://forum.ninox.de (Performance Documentation)

### Server Execution
```ninox
do as server
  "Code executed on server";
end
```

**Source**: Ninox Documentation

### Deferred Execution
```ninox
do as deferred
  "Code executed deferred";
end
```

**Source**: Ninox Documentation

---

## Important Notes

1. **Performance**: Prefer `select ... where` instead of `select` with subsequent filtering
2. **Transactions**: Use `do as transaction` for multiple write operations
3. **Source Citation**: Always provide source when using a function

---

**Sources**:
- Official Documentation: https://forum.ninox.de/category/docs
- Ninox Tutorials: https://ninox.com/de/tutorials
