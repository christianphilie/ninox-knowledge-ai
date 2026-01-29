# Relations and for Loops in Ninox

## Overview

This document explains the correct handling of relations and for loops in Ninox scripts.

**Source**: https://forum.ninox.de/category/docs

---

## What Are Relations?

Relations are linked tables in Ninox. If a table `Invoices` has a relation `'Invoice Positions'`, this relation automatically shows only the positions that belong to the current invoice.

**Important**: Relation fields (relations) are **not tables** and cannot be used directly in `select` statements. Use the name of the underlying table instead.

**Example**:
- ❌ WRONG: `select 'Pos Products /Services' where ...` (relation field)
- ✅ CORRECT: `select 'Pos Products' where ...` (table)

---

## For Loops with Relations

### ✅ CORRECT: Direct Iteration over Relations

```ninox
"Relation is already context-filtered";
"IMPORTANT: Store this in variable to avoid context problems";
let my := this;
for i in 'Invoice Positions' do
  "'Invoice Positions' automatically shows only positions of 'my'";
  "Code";
end
```

**Reason**: 
- Relations are already filtered by context. When you use `my.'Invoice Positions'`, it shows only the positions of the current invoice.
- **Important**: `let my := this;` reliably stores the context and avoids problems with `this` in loops or more complex operations. See also `references/common-mistakes.md` - Mistake 13.

---

## For Loops with Select Statements

### ✅ CORRECT: Select Before Loop

```ninox
"First select, then iterate";
let positions := select 'Invoice Positions' where Invoices = myID order by Pos;
for i in positions do
  "Code";
end
```

**Reason**: `select` creates a filtered and sorted collection that can then be iterated over.

---

## ❌ WRONG: Where Clause Directly in for Loop

```ninox
"Does NOT work in Ninox";
for i in select 'Invoice Positions' where Invoices = myID order by Pos do
  "Code";
end
```

**Reason**: `for` loops cannot be directly combined with `select ... where`. The syntax `for ... in select ... where ...` is not supported.

---

## When to Use Relations?

### Use relations when:
- You want to iterate over all linked records of a current record
- Filtering happens automatically through context
- You don't need additional sorting

**Example**:
```ninox
let my := this;
for position in 'Invoice Positions' do
  "Process all positions of this invoice";
end
```

---

## When to Use Select?

### Use select when:
- You need additional filtering (not just context filtering)
- You need sorting (`order by`)
- You need limiting (`limit`)
- You want to iterate over records from different contexts

**Example**:
```ninox
"All positions of a specific invoice, sorted";
let positions := select 'Invoice Positions' where Invoices = myID order by 'Pos Nr';
for i in positions do
  "Processing";
end
```

---

## Relation Fields vs. Tables in select

### ❌ WRONG: Use Relation Field in select
```ninox
"Relation field cannot be used directly in select";
let articlePositions := select 'Pos Products /Services' where ServiceReport = thisServiceReport;
```

**Problem**: `'Pos Products /Services'` is a relation field (relation), not a table. Relation fields only point to records in other tables and cannot be used directly in `select`.

### ✅ CORRECT: Use Table
```ninox
"Use the underlying table";
let articlePositions := select 'Pos Products' where ServiceReport = thisServiceReport;
```

**Reason**: 
- `select` requires the name of the actual table
- The table contains the records, the relation field only points to them
- With `where` you can then filter by the linked record

**When to use which**:
- **Relation field**: Directly in `for` loops: `for pos in 'Pos Products /Services' do`
- **Table**: In `select` statements: `select 'Pos Products' where ServiceReport = ...`

---

## Common Mistakes

### Mistake 1: Where in for Loop
```ninox
"❌ WRONG";
for i in select Table where Condition do
end
```

```ninox
"✅ CORRECT";
let filtered := select Table where Condition;
for i in filtered do
end
```

### Mistake 2: Combine Relation with where
```ninox
"❌ WRONG - Relations don't have a where parameter";
for i in 'Invoice Positions' where Condition do
end
```

```ninox
"✅ CORRECT - First select with where, then iterate";
let filtered := select 'Invoice Positions' where Condition;
for i in filtered do
end
```

### Mistake 3: Relation Field Instead of Table in select
```ninox
"❌ WRONG - Relation field cannot be used in select";
let positions := select 'Pos Products /Services' where ServiceReport = my;
```

```ninox
"✅ CORRECT - Use the table";
let positions := select 'Pos Products' where ServiceReport = my;
```

---

## Best Practices

1. **With Relations**: Iterate directly over the relation
   ```ninox
   for item in Relation do
   ```

2. **With Select**: First select, then iterate
   ```ninox
   let collection := select Table where Condition;
   for item in collection do
   ```

3. **Performance**: Use `limit` for large datasets
   ```ninox
   let collection := select Table where Condition limit 100;
   for item in collection do
   ```

---

## Summary

| Situation | Syntax |
|-----------|--------|
| Relation in current context | `for i in 'Relation' do` |
| Filtered records | `let filtered := select Table where Condition; for i in filtered do` |
| Sorted records | `let sorted := select Table order by Field; for i in sorted do` |
| Limited records | `let limited := select Table limit 10; for i in limited do` |
| **Important**: Relation field in select | `select Table where ...` (not `select 'Relation Field'`) |

**Important**: `for ... in select ... where ...` does **NOT** work in Ninox!

---

**Sources**:
- Official Documentation: https://forum.ninox.de/category/docs
- Common Mistakes: `references/common-mistakes.md`
