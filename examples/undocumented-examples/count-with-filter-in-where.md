# Beispiel: count() mit []-Filter in where-Klausel

## ⚠️ Undokumentiert aber funktionierend

### Verschachtelte count() mit []-Filter

```ninox
"⚠️ Nicht in offizieller Dokumentation, aber funktioniert";
"Findet alle Aufträge mit mindestens 5 nicht abgerechneten Serviceberichten";
select 'Aufträge' where count(Servicebericht[Abgerechnet = false]) >= 5
```

**Erklärung**:
- `Servicebericht` ist ein Relation-Feld (1:N Beziehung zu Aufträge)
- `[Abgerechnet = false]` filtert die Relation-Datensätze
- `count()` zählt die gefilterten Datensätze
- `>= 5` prüft, ob mindestens 5 Datensätze vorhanden sind

**Verwendung**:
- Funktioniert mit Relation-Feldern (1:N Beziehungen)
- Kann auch mit anderen Vergleichsoperatoren verwendet werden (`=`, `>`, `<`, `<=`, `>=`)
- Nützlich für komplexe Filterbedingungen basierend auf Relation-Daten

**Performance-Hinweis**:
- Performance kann bei großen Relationen beeinträchtigt sein
- Teste bei großen Datenmengen die Performance

---

## Weitere Beispiele

### Mit Gleichheitsprüfung
```ninox
"⚠️ Nicht in offizieller Dokumentation, aber funktioniert";
"Findet Kunden mit genau 3 Bestellungen";
select 'Kunden' where count(Bestellung) = 3
```

### Mit mehreren Bedingungen
```ninox
"⚠️ Nicht in offizieller Dokumentation, aber funktioniert";
"Findet Aufträge mit mindestens 10 offenen Positionen";
select 'Aufträge' where count(Position[Status = "Offen"]) >= 10
```

---

**Quellen**:
- Undocumented Features: `rules/undocumented-features.md`
- Performance-Hinweise: `rules/performance-rules.md`
