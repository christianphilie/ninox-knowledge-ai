# Pattern: Select Optimization

## Documented Pattern

### Select with Where (preferred)
```ninox
"Documented: Select with Where clause";
"Source: https://ninox.com/de/tutorials/advanced-tutorial-2";
let openOrders := select Orders where Status = "Open";
```

**Why documentation-compliant**:
- Uses documented `select ... where` syntax
- Performance-optimized (filters during selection)
- Source provided

---

## Comparison: Where vs. Subsequent Filtering

### ✅ PREFERRED: Select with Where
```ninox
"Good: Filters during selection (server-side)";
let orders := select Orders where Status = "Open";
```

### ❌ AVOID: Subsequent Filtering
```ninox
"Bad: Fetches all records, then filtering (client-side)";
let allOrders := select Orders;
let filtered := allOrders[Status = "Open"];
```

**Reason**: `where` filters during selection, which means less data transfer.

---

## Select Optimizations

### With Order By
```ninox
let recentOrders := select Orders order by Date desc limit 10;
```

### With Limit
```ninox
let topOrders := select Orders order by Amount desc limit 5;
```

### Combination
```ninox
let recentOpenOrders := select Orders 
  where Status = "Open" 
  order by Date desc 
  limit 10;
```

---

## Performance Tips

1. **Always use `where`** when possible
2. **Use `limit`** for large datasets
3. **Avoid `select` in loops**
4. **Avoid subsequent filtering**

---

**Sources**:
- Performance Rules: `references/performance-rules.md`
- Common Mistakes: `references/common-mistakes.md`
- Official Documentation: https://forum.ninox.de/category/docs
