# Pattern: Aggregate Functions Instead of Loops

## Documented Pattern

### Sum with Aggregate Function
```ninox
"Documented: Aggregate function instead of loop";
"Source: Ninox Documentation - Functions";
let totalAmount := sum(select Orders[Amount]);
```

**Why documentation-compliant**:
- Uses documented `sum()` function
- Performance-optimized
- Source provided

---

## Comparison: Loop vs. Aggregate Function

### ❌ BAD: Loop
```ninox
"Bad: Calculate sum in loop";
let total := 0;
for order in select Orders do
  total := total + order.Amount;
end
```

### ✅ GOOD: Aggregate Function
```ninox
"Good: Use aggregate function";
let total := sum(select Orders[Amount]);
```

**Reason**: Aggregate functions are optimized and faster.

---

## Available Aggregate Functions

- `sum(collection)` - Sum
- `count(collection)` - Count
- `average(collection)` - Average
- `min(collection)` - Minimum
- `max(collection)` - Maximum
- `first(collection)` - First record
- `last(collection)` - Last record

---

**Sources**:
- Function Whitelist: `references/function-whitelist.md`
- Performance Rules: `references/performance-rules.md`
- Official Documentation: https://forum.ninox.de/category/docs
