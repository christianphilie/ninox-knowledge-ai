# Pattern: Transaction Blocks

## Dokumentiertes Pattern

### Mehrere Schreiboperationen in Transaction
```ninox
"Dokumentiert: Transaction Block für mehrere Schreiboperationen";
"Quelle: https://forum.ninox.de (Performance-Dokumentation)";
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

**Warum dokumentationskonform**:
- Verwendet dokumentierte `do as transaction` Syntax
- Performance-optimiert (reduziert Datenbankzugriffe)
- Quelle angegeben

---

## Verwendung

**Wann verwenden**:
- Bei mehreren Schreiboperationen (create, update, delete)
- Wenn mehrere Datensätze gleichzeitig geändert werden sollen
- Für bessere Performance

**Vorteile**:
- Reduziert Anzahl der Datenbankzugriffe
- Bessere Performance
- Atomare Operationen

---

**Quellen**:
- Performance-Regeln: `rules/performance-rules.md`
- Offizielle Dokumentation: https://forum.ninox.de/category/docs
