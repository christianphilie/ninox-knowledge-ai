# Example: Basic Select Statements

## Documented Example

### Select with Where
```ninox
"Documented: Select with Where clause";
"Source: https://ninox.com/de/tutorials/advanced-tutorial-2";
let openOrders := select Orders where Status = "Open";
```

**Why documentation-compliant**:
- Uses documented `select ... where` syntax
- Source provided
- Performance-optimized (filters during selection)

---

### Select with Order By
```ninox
"Documented: Select with sorting";
"Source: https://ninox.com/de/tutorials/advanced-tutorial-2";
let recentOrders := select Orders order by Date desc limit 10;
```

**Why documentation-compliant**:
- Uses documented `order by` syntax
- Uses `limit` for performance
- Source provided

---

### Aggregate Functions
```ninox
"Documented: Aggregate functions";
"Source: Ninox Documentation - Functions";
let totalAmount := sum(select Orders[Amount]);
let orderCount := count(select Orders);
let averageAmount := average(select Orders[Amount]);
```

**Why documentation-compliant**:
- Uses documented aggregate functions (`sum`, `count`, `average`)
- Source provided

---

## Performance-Optimized Example

### Transaction Block
```ninox
"Documented: Transaction block for multiple write operations";
"Source: https://forum.ninox.de (Performance Documentation)";
do as transaction
  let newOrder := create Orders;
  newOrder.Customer := customer;
  newOrder.Amount := amount;
  newOrder.Date := today();
end
```

**Why documentation-compliant**:
- Uses documented `do as transaction` syntax
- Performance-optimized (reduces database accesses)
- Source provided

---

## Sources

- Official Documentation: https://forum.ninox.de/category/docs
- Ninox Tutorials: https://ninox.com/de/tutorials
