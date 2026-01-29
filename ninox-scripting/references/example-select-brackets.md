# Example: Select with Square Brackets

## ⚠️ Undocumented but Working

### Select with [] Filter
```ninox
"⚠️ Not in official documentation, but works";
"Alternative to: select Orders where Status = 'Open'";
let openOrders := select Orders[Status = "Open"];
```

**Notes**:
- Works, but performance difference from `where`
- `select ... where` filters during selection (server-side)
- `select ...[...]` fetches all records first, then filtering (client-side)
- **Prefer `where` for better performance**

**Source**: Community examples in forum

---

### Combination with Other Operations
```ninox
"⚠️ Not in official documentation, but works";
let filteredOrders := select Orders[Status = "Open"][Amount > 100];
```

**Notes**:
- Multiple filtering with `[]` possible
- But: `select Orders where Status = "Open" and Amount > 100` is better

---

## Comparison: Where vs. Square Brackets

### With Where (preferred)
```ninox
"✅ PREFERRED: Filters during selection";
let orders := select Orders where Status = "Open";
```

### With Square Brackets
```ninox
"⚠️ Works, but less performant";
let orders := select Orders[Status = "Open"];
```

---

## Important

- This syntax works but is not officially documented
- Prefer `select ... where` for better performance
- Always label with `⚠️ Not in official documentation, but works`

---

**Sources**:
- Community Examples: https://forum.ninox.de
- Performance Notes: `references/performance-rules.md`
