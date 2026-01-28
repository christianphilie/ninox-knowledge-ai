# Beispiel: Select mit eckigen Klammern

## ⚠️ Undokumentiert aber funktionierend

### Select mit []-Filter
```ninox
"⚠️ Nicht in offizieller Dokumentation, aber funktioniert";
"Alternative zu: select Orders where Status = 'Open'";
let openOrders := select Orders[Status = "Open"];
```

**Hinweise**:
- Funktioniert, aber Performance-Unterschied zu `where`
- `select ... where` filtert bereits bei der Selektion (serverseitig)
- `select ...[...]` holt zuerst alle Datensätze, dann Filterung (clientseitig)
- **Bevorzuge `where` für bessere Performance**

**Quelle**: Community-Beispiele im Forum

---

### Kombination mit anderen Operationen
```ninox
"⚠️ Nicht in offizieller Dokumentation, aber funktioniert";
let filteredOrders := select Orders[Status = "Open"][Amount > 100];
```

**Hinweise**:
- Mehrfache Filterung mit `[]` möglich
- Aber: `select Orders where Status = "Open" and Amount > 100` ist besser

---

## Vergleich: Where vs. Eckige Klammern

### Mit Where (bevorzugt)
```ninox
"✅ BEVORZUGT: Filtert bereits bei Selektion";
let orders := select Orders where Status = "Open";
```

### Mit Eckigen Klammern
```ninox
"⚠️ Funktioniert, aber weniger performant";
let orders := select Orders[Status = "Open"];
```

---

## Wichtig

- Diese Syntax funktioniert, ist aber nicht offiziell dokumentiert
- Bevorzuge `select ... where` für bessere Performance
- Kennzeichne immer mit `⚠️ Nicht in offizieller Dokumentation, aber funktioniert`

---

**Quellen**:
- Community-Beispiele: https://forum.ninox.de
- Performance-Hinweise: `rules/performance-rules.md`
