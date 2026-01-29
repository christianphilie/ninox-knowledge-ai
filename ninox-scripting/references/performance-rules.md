# Performance Rules for Ninox Scripting

## Overview

These rules describe performance optimizations for Ninox scripts.

**Source**: https://forum.ninox.de/category/docs

---

## Rule 1: Prefer Select with Where

### ✅ CORRECT
```ninox
"Filters during selection (server-side)";
select Orders where Status = "Open"
```

### ❌ WRONG
```ninox
"Fetches all records, then filtering (client-side)";
let orders := select Orders;
let filtered := orders[Status = "Open"];
```

**Reason**: `where` filters during selection, which means less data transfer.

---

## Rule 2: Use Transaction Blocks

### ✅ CORRECT
```ninox
do as transaction
  record1.field := value1;
  record2.field := value2;
  record3.field := value3;
end
```

### ❌ WRONG
```ninox
"Each operation individually";
record1.field := value1;
record2.field := value2;
record3.field := value3;
```

**Reason**: Transaction blocks reduce the number of database accesses.

---

## Rule 3: Avoid Select in Loops

### ❌ WRONG
```ninox
"Select in each iteration";
for customer in select Customers do
  let orders := select Orders where Customer = customer;
end
```

### ✅ CORRECT
```ninox
"Select once";
let allOrders := select Orders;
for customer in select Customers do
  let orders := allOrders[Customer = customer];
end
```

**Reason**: Each `select` is a database access. Avoid multiple accesses.

---

## Rule 4: Aggregate Functions Instead of Loops

### ❌ WRONG
```ninox
"Calculate sum in loop";
let total := 0;
for order in select Orders do
  total := total + order.Amount;
end
```

### ✅ CORRECT
```ninox
"Use aggregate function";
let total := sum(select Orders[Amount]);
```

**Reason**: Aggregate functions are optimized and faster.

---

## Rule 5: Limit for Large Datasets

### ✅ CORRECT
```ninox
"Only load needed records";
select Orders order by Date desc limit 10
```

### ❌ WRONG
```ninox
"Load all records";
select Orders order by Date desc
```

---

## Performance Block Types

### do as transaction
**Usage**: For multiple write operations
```ninox
do as transaction
  "Multiple write operations";
end
```

### do as server
**Usage**: Execute code on server side
```ninox
do as server
  "Server-side execution";
end
```

### do as deferred
**Usage**: Deferred execution
```ninox
do as deferred
  "Deferred execution";
end
```

---

## Checklist

Before every script check:

- [ ] Use `select ... where` instead of subsequent filtering
- [ ] Use `do as transaction` for multiple write operations
- [ ] Avoid `select` in loops
- [ ] Use aggregate functions instead of loops
- [ ] Use `limit` for large datasets
- [ ] Avoid nested `select` statements

---

**Sources**:
- Official Documentation: https://forum.ninox.de/category/docs
- Performance Discussions: https://forum.ninox.de
