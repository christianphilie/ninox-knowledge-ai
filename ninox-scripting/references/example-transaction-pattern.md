# Pattern: Transaction Blocks

## Documented Pattern

### Multiple Write Operations in Transaction
```ninox
"Documented: Transaction block for multiple write operations";
"Source: https://forum.ninox.de (Performance Documentation)";
do as transaction
  let newOrder := create Orders;
  newOrder.Customer := customer;
  newOrder.Amount := amount;
  newOrder.Date := today();
  
  let newItem := create OrderItems;
  newItem.Order := newOrder;
  newItem.Product := product;
  newItem.Quantity := quantity;
end
```

**Why documentation-compliant**:
- Uses documented `do as transaction` syntax
- Performance-optimized (reduces database accesses)
- Source provided

---

## Usage

**When to use**:
- For multiple write operations (create, update, delete)
- When multiple records should be changed simultaneously
- For better performance

**Advantages**:
- Reduces number of database accesses
- Better performance
- Atomic operations

---

**Sources**:
- Performance Rules: `references/performance-rules.md`
- Official Documentation: https://forum.ninox.de/category/docs
