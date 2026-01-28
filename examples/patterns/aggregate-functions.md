# Pattern: Aggregatfunktionen statt Loops

## Dokumentiertes Pattern

### Summe mit Aggregatfunktion
```ninox
"Dokumentiert: Aggregatfunktion statt Loop";
"Quelle: Ninox Dokumentation - Funktionen";
let totalAmount := sum(select Orders[Amount]);
```

**Warum dokumentationskonform**:
- Verwendet dokumentierte `sum()` Funktion
- Performance-optimiert
- Quelle angegeben

---

## Vergleich: Loop vs. Aggregatfunktion

### ❌ SCHLECHT: Loop
```ninox
"Schlecht: Summe in Loop berechnen";
let total := 0;
for order in select Orders do
  total := total + order.Amount;
end
```

### ✅ GUT: Aggregatfunktion
```ninox
"Gut: Aggregatfunktion verwenden";
let total := sum(select Orders[Amount]);
```

**Grund**: Aggregatfunktionen sind optimiert und schneller.

---

## Verfügbare Aggregatfunktionen

- `sum(collection)` - Summe
- `count(collection)` - Anzahl
- `average(collection)` - Durchschnitt
- `min(collection)` - Minimum
- `max(collection)` - Maximum
- `first(collection)` - Erster Datensatz
- `last(collection)` - Letzter Datensatz

---

**Quellen**:
- Function Whitelist: `rules/function-whitelist.md`
- Performance-Regeln: `rules/performance-rules.md`
- Offizielle Dokumentation: https://forum.ninox.de/category/docs
