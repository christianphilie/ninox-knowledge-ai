# Ninox Scripting - Performance Optimization

## Overview

This documentation describes best practices for performance optimization in Ninox scripts.

**Source**: https://forum.ninox.de/category/docs

---

## Most Important Rules

### 1. Select Optimization

#### ✅ PREFERRED: Select with Where
```ninox
"Good: Filters during selection (server-side)";
select Orders where Status = "Open"
```

#### ⚠️ AVOID: Select without Where, Then Filtering
```ninox
"Bad: Fetches all records, then filtering (client-side)";
let orders := select Orders;
let filtered := orders[Status = "Open"];
```

**Reason**: `where` filters during selection, which means less data transfer and better performance.

**Source**: https://forum.ninox.de/t/h7yxvgh

---

### 2. Transaction Blocks

#### ✅ PREFERRED: Multiple Operations in Transaction
```ninox
do as transaction
  record1.field := value1;
  record2.field := value2;
  record3.field := value3;
end
```

#### ❌ AVOID: Individual Operations
```ninox
"Bad: Each operation individually";
record1.field := value1;
record2.field := value2;
record3.field := value3;
```

**Reason**: Transaction blocks reduce the number of database accesses.

**Source**: https://forum.ninox.de (Performance Documentation)

---

### 3. Avoid Nested Selects

#### ❌ AVOID: Select in Loop
```ninox
"Bad: Select in each iteration";
for customer in select Customers do
  let orders := select Orders where Customer = customer;
  "Processing";
end
```

#### ✅ PREFERRED: Select Once, Then Processing
```ninox
"Good: Select once, then processing";
let allOrders := select Orders;
for customer in select Customers do
  let orders := allOrders[Customer = customer];
  "Processing";
end
```

**Reason**: Each `select` is a database access. Avoid multiple accesses.

---

### 4. Aggregate Functions Instead of Loops

#### ❌ AVOID: Calculate Sum in Loop
```ninox
"Bad: Loop for sum";
let total := 0;
for order in select Orders do
  total := total + order.Amount;
end
```

#### ✅ PREFERRED: Use Aggregate Function
```ninox
"Good: Aggregate function";
let total := sum(select Orders[Amount]);
```

**Reason**: Aggregate functions are optimized and faster.

---

### 5. Limit for Large Datasets

#### ✅ PREFERRED: Use Limit
```ninox
"Good: Only load needed records";
select Orders order by Date desc limit 10
```

#### ❌ AVOID: Load All Records
```ninox
"Bad: Load all records";
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

**Source**: Ninox Documentation

---

## Performance Checklist

Before every script check:

- [ ] Use `select ... where` instead of subsequent filtering
- [ ] Use `do as transaction` for multiple write operations
- [ ] Avoid `select` in loops
- [ ] Use aggregate functions instead of loops
- [ ] Use `limit` for large datasets
- [ ] Avoid nested `select` statements

---

## Common Performance Mistakes

### Mistake 1: Select in Loop
```ninox
"❌ BAD";
for customer in select Customers do
  let orders := select Orders where Customer = customer;
end
```

### Mistake 2: No Transaction for Multiple Write Operations
```ninox
"❌ BAD";
record1.field := value1;
record2.field := value2;
record3.field := value3;
```

### Mistake 3: Subsequent Filtering Instead of Where
```ninox
"❌ BAD";
let allOrders := select Orders;
let filtered := allOrders[Status = "Open"];
```

---

**Sources**:
- Official Documentation: https://forum.ninox.de/category/docs
- Performance Discussions: https://forum.ninox.de
