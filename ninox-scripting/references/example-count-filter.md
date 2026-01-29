# Example: count() with [] Filter in where Clause

## ⚠️ Undocumented but Working

### Nested count() with [] Filter

```ninox
"⚠️ Not in official documentation, but works";
"Finds all orders with at least 5 unbilled service reports";
select 'Orders' where count(ServiceReport[Billed = false]) >= 5
```

**Explanation**:
- `ServiceReport` is a relation field (1:N relationship to Orders)
- `[Billed = false]` filters the relation records
- `count()` counts the filtered records
- `>= 5` checks if at least 5 records exist

**Usage**:
- Works with relation fields (1:N relationships)
- Can also be used with other comparison operators (`=`, `>`, `<`, `<=`, `>=`)
- Useful for complex filter conditions based on relation data

**Performance Note**:
- Performance can be affected with large relations
- Test performance with large datasets

---

## Further Examples

### With Equality Check
```ninox
"⚠️ Not in official documentation, but works";
"Finds customers with exactly 3 orders";
select 'Customers' where count(Order) = 3
```

### With Multiple Conditions
```ninox
"⚠️ Not in official documentation, but works";
"Finds orders with at least 10 open positions";
select 'Orders' where count(Position[Status = "Open"]) >= 10
```

---

**Sources**:
- Undocumented Features: `references/undocumented-features.md`
- Performance Notes: `references/performance-rules.md`
