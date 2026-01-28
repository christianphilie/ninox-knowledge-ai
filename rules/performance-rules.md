# Performance-Regeln für Ninox-Scripting

## Übersicht

Diese Regeln beschreiben Performance-Optimierungen für Ninox-Skripte.

**Quelle**: https://forum.ninox.de/category/docs

---

## Regel 1: Select mit Where bevorzugen

### ✅ RICHTIG
```ninox
"Filtert bereits bei der Selektion (serverseitig)";
select Orders where Status = "Open"
```

### ❌ FALSCH
```ninox
"Holt alle Datensätze, dann Filterung (clientseitig)";
let orders := select Orders;
let filtered := orders[Status = "Open"];
```

**Grund**: `where` filtert bereits bei der Selektion, was weniger Datenverkehr bedeutet.

---

## Regel 2: Transaction Blocks verwenden

### ✅ RICHTIG
```ninox
do as transaction
  record1.field := value1;
  record2.field := value2;
  record3.field := value3;
end
```

### ❌ FALSCH
```ninox
"Jede Operation einzeln";
record1.field := value1;
record2.field := value2;
record3.field := value3;
```

**Grund**: Transaction Blocks reduzieren die Anzahl der Datenbankzugriffe.

---

## Regel 3: Vermeide Select in Schleifen

### ❌ FALSCH
```ninox
"Select in jeder Iteration";
for customer in select Customers do
  let orders := select Orders where Customer = customer;
end
```

### ✅ RICHTIG
```ninox
"Select einmalig";
let allOrders := select Orders;
for customer in select Customers do
  let orders := allOrders[Customer = customer];
end
```

**Grund**: Jeder `select` ist ein Datenbankzugriff. Mehrfache Zugriffe vermeiden.

---

## Regel 4: Aggregatfunktionen statt Loops

### ❌ FALSCH
```ninox
"Summe in Loop berechnen";
let total := 0;
for order in select Orders do
  total := total + order.Amount;
end
```

### ✅ RICHTIG
```ninox
"Aggregatfunktion verwenden";
let total := sum(select Orders[Amount]);
```

**Grund**: Aggregatfunktionen sind optimiert und schneller.

---

## Regel 5: Limit bei großen Datensätzen

### ✅ RICHTIG
```ninox
"Nur benötigte Datensätze laden";
select Orders order by Date desc limit 10
```

### ❌ FALSCH
```ninox
"Alle Datensätze laden";
select Orders order by Date desc
```

---

## Performance-Block-Typen

### do as transaction
**Verwendung**: Bei mehreren Schreiboperationen
```ninox
do as transaction
  "Mehrere Schreiboperationen";
end
```

### do as server
**Verwendung**: Code auf Server-Seite ausführen
```ninox
do as server
  "Server-seitige Ausführung";
end
```

### do as deferred
**Verwendung**: Verzögerte Ausführung
```ninox
do as deferred
  "Verzögerte Ausführung";
end
```

---

## Checkliste

Vor jedem Skript prüfen:

- [ ] Verwende `select ... where` statt nachträglicher Filterung
- [ ] Verwende `do as transaction` bei mehreren Schreiboperationen
- [ ] Vermeide `select` in Schleifen
- [ ] Verwende Aggregatfunktionen statt Loops
- [ ] Verwende `limit` bei großen Datensätzen
- [ ] Vermeide verschachtelte `select`-Anweisungen

---

**Quellen**:
- Offizielle Dokumentation: https://forum.ninox.de/category/docs
- Performance-Diskussionen: https://forum.ninox.de
