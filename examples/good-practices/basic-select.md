# Beispiel: Basis Select-Anweisungen

## Dokumentiertes Beispiel

### Select mit Where
```ninox
"Dokumentiert: Select mit Where-Klausel";
"Quelle: https://ninox.com/de/tutorials/advanced-tutorial-2";
let openOrders := select Orders where Status = "Open";
```

**Warum dokumentationskonform**:
- Verwendet dokumentierte `select ... where` Syntax
- Quelle angegeben
- Performance-optimiert (filtert bereits bei Selektion)

---

### Select mit Order By
```ninox
"Dokumentiert: Select mit Sortierung";
"Quelle: https://ninox.com/de/tutorials/advanced-tutorial-2";
let recentOrders := select Orders order by Date desc limit 10;
```

**Warum dokumentationskonform**:
- Verwendet dokumentierte `order by` Syntax
- Verwendet `limit` für Performance
- Quelle angegeben

---

### Aggregatfunktionen
```ninox
"Dokumentiert: Aggregatfunktionen";
"Quelle: Ninox Dokumentation - Funktionen";
let totalAmount := sum(select Orders[Amount]);
let orderCount := count(select Orders);
let averageAmount := average(select Orders[Amount]);
```

**Warum dokumentationskonform**:
- Verwendet dokumentierte Aggregatfunktionen (`sum`, `count`, `average`)
- Quelle angegeben

---

## Performance-Optimiertes Beispiel

### Transaction Block
```ninox
"Dokumentiert: Transaction Block für mehrere Schreiboperationen";
"Quelle: https://forum.ninox.de (Performance-Dokumentation)";
do as transaction
  let newOrder := create Orders;
  newOrder.Customer := customer;
  newOrder.Amount := amount;
  newOrder.Date := today();
end
```

**Warum dokumentationskonform**:
- Verwendet dokumentierte `do as transaction` Syntax
- Performance-optimiert (reduziert Datenbankzugriffe)
- Quelle angegeben

---

## Quellen

- Offizielle Dokumentation: https://forum.ninox.de/category/docs
- Ninox Tutorials: https://ninox.com/de/tutorials
