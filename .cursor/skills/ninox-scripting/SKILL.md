---
name: ninox-scripting
description: Expert knowledge for scripting in Ninox databases, preventing hallucinations and enforcing project rules. Use when writing, debugging, or explaining Ninox scripts (NX).
---

# Ninox Scripting Skill

Dieses Skill MUSS verwendet werden, wenn der Benutzer nach dem Schreiben, Debuggen oder Erklären von Ninox-Skripten (NX) fragt.

## ⚠️ KRITISCHE REGELN

Du musst strikt den RAG-Regeln des Projekts folgen, um "Halluzinationen" (Erfinden von nicht existierenden Funktionen) zu vermeiden.

1. **IMMER** `rules/function-whitelist.md` lesen, bevor Code geschrieben wird. Wenn eine Funktion nicht in dieser Liste steht, **VERWENDE SIE NICHT**, es sei denn, du verifizierst sie selbst in der offiziellen Dokumentation.
2. **IMMER** `rules/forbidden-patterns.md` lesen, um häufige Fehler zu vermeiden (z.B. Verwendung von JS-Array-Methoden wie `map`, `filter`, etc., die in Ninox NICHT existieren).
3. **KONTEXT-ABFRAGEN**: Wenn Feldnamen, Tabellennamen oder Relationen unklar sind, **FRAGE NACH** statt zu raten. Siehe `rules/context-queries.md`.
4. **ALLE REGELN KONSULTIEREN**: Überprüfe alle relevanten Dokumente im `rules/` Verzeichnis, bevor Code geschrieben wird. Siehe Ressourcen-Abschnitt unten für alle verfügbaren Regel-Dokumente.

## Ressourcen

* [Function Whitelist](rules/function-whitelist.md)
* [Forbidden Patterns](rules/forbidden-patterns.md)
* [Common Mistakes](rules/common-mistakes.md)
* [Context Queries](rules/context-queries.md) - Wann nachfragen statt raten
* [Relations and Loops](rules/relations-and-loops.md) - Korrekte Verwendung von Relationen und for-Schleifen
* [Performance Rules](rules/performance-rules.md)
* [Undocumented Features](rules/undocumented-features.md)
* [Style Guide](rules/style-guide.md)
