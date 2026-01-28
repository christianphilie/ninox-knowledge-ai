# Undokumentierte aber funktionierende Features

Diese Features sind **nicht** in der offiziellen Dokumentation beschrieben, funktionieren aber nachweislich. Sie dürfen verwendet werden, müssen aber **immer** mit entsprechender Kennzeichnung versehen werden.

## Verwendungsregel

**OBLIGATORISCHE KENNZEICHNUNG**: Immer mit Kommentar markieren:
```
⚠️ Nicht in offizieller Dokumentation, aber funktioniert
```

## Liste undokumentierter Features

### 1. Eckige Klammern `[]` als Filter nach `select`

**Beschreibung**: Nach einem `select` kann mit eckigen Klammern `[]` zusätzlich gefiltert werden.

**Beispiel**:
```ninox
"⚠️ Nicht in offizieller Dokumentation, aber funktioniert";
let records := select Tabelle[ Feld = "Wert" ];
```

**Hinweis**: 
- Funktioniert, aber Performance-Unterschied zu `select ... where`
- `select Tabelle where Feld = "Wert"` filtert bereits bei der Selektion (serverseitig)
- `select Tabelle[ Feld = "Wert" ]` holt zuerst alle Datensätze, dann Filterung (clientseitig)
- Bevorzuge `where` für bessere Performance

**Quelle**: Community-Beispiele im Forum

---

### 1a. Eckige Klammern `[]` als Filter direkt auf Relationen

**Beschreibung**: Relationen können direkt mit eckigen Klammern `[]` gefiltert werden, ohne `select` zu verwenden.

**Syntax**:
```ninox
"⚠️ Nicht in offizieller Dokumentation, aber funktioniert";
Relation[ Feld = Wert ]
```

**Beispiel**:
```ninox
"⚠️ Nicht in offizieller Dokumentation, aber funktioniert";
let my := this;
let gefilterteLagerbuchungen := my.Produkt.Lagerbuchungen[Lager = my.'von Quell-Lager'];
let ersteBuchung := first(gefilterteLagerbuchungen);
```

**Erklärung**:
- `my.Produkt.Lagerbuchungen` ist eine Relation (1:N Beziehung)
- `[Lager = my.'von Quell-Lager']` filtert die Relation direkt
- Das Ergebnis kann mit `first()`, `last()`, `count()` etc. weiterverarbeitet werden
- Direkter Feldzugriff möglich: `first(Relation[Filter]).Feld`

**Hinweise**:
- Funktioniert direkt auf Relationen, nicht nur nach `select`
- Sehr kompakt und effizient
- Kann mit `first()`, `last()`, `count()` kombiniert werden
- Performance kann bei sehr großen Relationen beeinträchtigt sein

**Unterschied zu `select Table[Filter]`**:
- `select Table[Filter]` funktioniert auf Tabellen
- `Relation[Filter]` funktioniert direkt auf Relationen (ohne `select`)
- Beide verwenden die gleiche `[]`-Syntax

**Quelle**: Getestet und funktionierend, aber nicht in offizieller Dokumentation gefunden

---

### 2. Verschachtelte `count()` mit `[]`-Filter in `where`-Klauseln

**Beschreibung**: In `where`-Klauseln kann `count()` mit eckigen Klammern `[]` verwendet werden, um Relation-Felder zu filtern und zu zählen.

**Syntax**:
```ninox
"⚠️ Nicht in offizieller Dokumentation, aber funktioniert";
select 'Tabelle' where count(RelationFeld[Bedingung]) >= Anzahl
```

**Beispiel**:
```ninox
"⚠️ Nicht in offizieller Dokumentation, aber funktioniert";
"Findet alle Aufträge mit mindestens 5 nicht abgerechneten Serviceberichten";
select 'Aufträge' where count(Servicebericht[Abgerechnet = false]) >= 5
```

**Erklärung**:
- `Servicebericht` ist ein Relation-Feld (1:N Beziehung)
- `[Abgerechnet = false]` filtert die Relation-Datensätze
- `count()` zählt die gefilterten Datensätze
- `>= 5` prüft, ob mindestens 5 Datensätze vorhanden sind

**Hinweise**:
- Funktioniert mit Relation-Feldern (1:N Beziehungen)
- `[]`-Filter wird innerhalb von `count()` angewendet
- Kann auch mit anderen Vergleichsoperatoren verwendet werden (`=`, `>`, `<`, `<=`, `>=`)
- Performance kann bei großen Relationen beeinträchtigt sein

**Quelle**: Getestet und funktionierend, aber nicht in offizieller Dokumentation gefunden

---

### 3. `cnt()` als Alternative zu `count()`

**Beschreibung**: `cnt()` ist ein Alias für `count()` und funktioniert identisch.

**Beispiel**:
```ninox
"⚠️ Nicht in offizieller Dokumentation, aber funktioniert";
let anzahl := cnt(select Bestellungen);
"Identisch zu: count(select Bestellungen)";
```

**Hinweise**:
- Funktioniert identisch wie `count()`
- Wird häufig in Community-Beispielen verwendet
- Beide Varianten sind austauschbar

**Quelle**: Community-Beispiele im Forum

---

### 4. `for i from start to end` - Bereichs-Schleife

**Beschreibung**: Schleife über einen numerischen Bereich ohne explizite Collection.

**Syntax**:
```ninox
"⚠️ Nicht in offizieller Dokumentation, aber funktioniert";
for i from start to end do
  "Code";
end
```

**Beispiel**:
```ninox
"⚠️ Nicht in offizieller Dokumentation, aber funktioniert";
"Iteriert von 7 bis 18 (inklusive)";
for hour from 7 to 18 do
  alert("Stunde: " + hour);
end
```

**Hinweise**:
- `start` und `end` müssen Zahlen sein
- Beide Grenzen sind inklusive
- Kann auch mit `for i from 0 to 5` verwendet werden

**Quelle**: Getestet und funktionierend, aber nicht in offizieller Dokumentation gefunden

---

### 5. `start()` und `endof()` für Datetime-Zeiträume

**Beschreibung**: Funktionen zum Extrahieren von Start- und Endzeitpunkt aus einem Datetime-Zeitraum.

**Syntax**:
```ninox
"⚠️ Nicht in offizieller Dokumentation, aber funktioniert";
start(datetime);
"Startzeitpunkt";
endof(datetime);
"Endzeitpunkt";
```

**Beispiel**:
```ninox
"⚠️ Nicht in offizieller Dokumentation, aber funktioniert";
let termin := item.Termin;
"Datetime-Zeitraum";
let startZeit := start(termin);
let endZeit := endof(termin);
let dauer := endZeit - startZeit;
```

**Hinweise**:
- Funktioniert mit Datetime-Feldern, die Zeiträume repräsentieren
- `start()` gibt den Beginn zurück
- `endof()` gibt das Ende zurück
- Nützlich für Terminplanung und Zeitberechnungen

**Quelle**: Getestet und funktionierend, aber nicht in offizieller Dokumentation gefunden

---

### 6. `extractx()` - Regex-basierte Extraktion

**Beschreibung**: Extrahiert Text mit regulären Ausdrücken und Ersetzungsmustern.

**Syntax**:
```ninox
"⚠️ Nicht in offizieller Dokumentation, aber funktioniert";
extractx(text, pattern, replacement)
```

**Beispiel**:
```ninox
"⚠️ Nicht in offizieller Dokumentation, aber funktioniert";
let layoutInfo := ";123:45,60;";
let layoutStr := extractx(layoutInfo, ";" + text(123) + ":(.+?);", "$1");
"Ergebnis: '45,60'";
```

**Hinweise**:
- Verwendet reguläre Ausdrücke
- `replacement` kann Ersetzungsmuster wie `$1`, `$2` enthalten
- Nützlich für komplexe Textverarbeitung

**Quelle**: Getestet und funktionierend, aber nicht in offizieller Dokumentation gefunden

---

### 7. `popupRecord()` - Datensatz in Popup öffnen

**Beschreibung**: Öffnet einen Datensatz in einem Popup-Fenster.

**Syntax**:
```ninox
"⚠️ Nicht in offizieller Dokumentation, aber funktioniert";
popupRecord(record)
```

**Beispiel**:
```ninox
"⚠️ Nicht in offizieller Dokumentation, aber funktioniert";
let newRecord := create 'Kunden / Kontakte';
popupRecord(newRecord);
```

**Hinweise**:
- Öffnet den Datensatz in einem Popup
- Nützlich für UI-Interaktionen
- Ähnlich zu `openRecord()`, aber als Popup

**Quelle**: Getestet und funktionierend, aber nicht in offizieller Dokumentation gefunden

---

### 8. `html()` - HTML-Ausgabe

**Beschreibung**: Gibt HTML-Code direkt aus, der im Browser gerendert wird.

**Syntax**:
```ninox
"⚠️ Nicht in offizieller Dokumentation, aber funktioniert";
html(htmlCode)
```

**Beispiel**:
```ninox
"⚠️ Nicht in offizieller Dokumentation, aber funktioniert";
html("<div style='color: red;'>Hallo Welt</div>");
```

**Hinweise**:
- Gibt HTML direkt aus
- Nützlich für komplexe UI-Elemente
- Kann CSS und JavaScript enthalten

**Quelle**: Getestet und funktionierend, aber nicht in offizieller Dokumentation gefunden

---

### 9. `color()` - Farbe eines Feldes abrufen

**Beschreibung**: Gibt die Farbe eines Feldes zurück (z.B. bei Auswahlfeldern mit Farbzuordnung).

**Syntax**:
```ninox
"⚠️ Nicht in offizieller Dokumentation, aber funktioniert";
color(field)
```

**Beispiel**:
```ninox
"⚠️ Nicht in offizieller Dokumentation, aber funktioniert";
let farbe := color(item.Terminart);
"Gibt z.B. 'blue' oder '#ff0000' zurück";
```

**Hinweise**:
- Funktioniert mit Feldern, die Farben zugeordnet haben
- Gibt Farbwert als Text zurück (z.B. "blue", "#ff0000")
- Nützlich für dynamische Formatierung

**Quelle**: Getestet und funktionierend, aber nicht in offizieller Dokumentation gefunden

---

### 10. `formatXML()` - XML formatieren

**Beschreibung**: Formatiert Daten als XML.

**Syntax**:
```ninox
"⚠️ Nicht in offizieller Dokumentation, aber funktioniert";
formatXML(data, pretty)
```

**Beispiel**:
```ninox
"⚠️ Nicht in offizieller Dokumentation, aber funktioniert";
let xml := formatXML(data, true);
"Formatiert Daten als XML mit Einrückung (pretty = true)";
```

**Hinweise**:
- `pretty` bestimmt, ob XML formatiert wird (true) oder kompakt (false)
- Nützlich für Datenexport und API-Integrationen

**Quelle**: Getestet und funktionierend, aber nicht in offizieller Dokumentation gefunden

---

### Weitere Features

Diese Liste wird kontinuierlich erweitert, wenn neue undokumentierte aber funktionierende Features entdeckt werden.

## Hinzufügen neuer Features

Wenn du ein neues undokumentiertes aber funktionierendes Feature entdeckst:

1. Teste es gründlich
2. Stelle sicher, dass es konsistent funktioniert
3. Dokumentiere es hier mit:
   - Beschreibung
   - Beispiel-Code
   - Hinweise zur Verwendung
   - Quelle (wenn verfügbar)

## Wichtig

- Diese Features können sich ändern oder entfernt werden
- Keine Garantie für zukünftige Kompatibilität
- Immer dokumentierte Alternative bevorzugen, wenn verfügbar
