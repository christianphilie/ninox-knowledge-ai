# Pattern: Select-Optimierung

## Dokumentiertes Pattern

### Select mit Where (bevorzugt)
```ninox
"Dokumentiert: Select mit Where-Klausel";
"Quelle: https://ninox.com/de/tutorials/advanced-tutorial-2";
let openOrders := select Orders where Status = "Open";
```

**Warum dokumentationskonform**:
- Verwendet dokumentierte `select ... where` Syntax
- Performance-optimiert (filtert bereits bei Selektion)
- Quelle angegeben

---

## Vergleich: Where vs. Nachträgliche Filterung

### ✅ BEVORZUGT: Select mit Where
```ninox
"Gut: Filtert bereits bei Selektion (serverseitig)";
let orders := select Orders where Status = "Open";
```

### ❌ VERMEIDEN: Nachträgliche Filterung
```ninox
"Schlecht: Holt alle Datensätze, dann Filterung (clientseitig)";
let allOrders := select Orders;
let filtered := allOrders[Status = "Open"];
```

**Grund**: `where` filtert bereits bei der Selektion, was weniger Datenverkehr bedeutet.

---

## Select-Optimierungen

### Mit Order By
```ninox
let recentOrders := select Orders order by Date desc limit 10;
```

### Mit Limit
```ninox
let topOrders := select Orders order by Amount desc limit 5;
```

### Kombination
```ninox
let recentOpenOrders := select Orders 
  where Status = "Open" 
  order by Date desc 
  limit 10;
```

---

## Performance-Tipps

1. **Immer `where` verwenden** wenn möglich
2. **`limit` verwenden** bei großen Datensätzen
3. **Vermeide `select` in Schleifen**
4. **Vermeide nachträgliche Filterung**

---

**Quellen**:
- Performance-Regeln: `rules/performance-rules.md`
- Common Mistakes: `rules/common-mistakes.md`
- Offizielle Dokumentation: https://forum.ninox.de/category/docs
