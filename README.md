# Ninox Knowledge Base für Cursor

Knowledge Base für **[Cursor](https://cursor.sh)**, die mittels RAG sicherstellt, dass mit Cursor generierte **[Ninox](https://ninox.com)**-Skripte nur dokumentierte Funktionen verwenden und keine erfundenen Features enthalten.

**Problem**: LLMs erfinden oft Funktionen oder übernehmen Syntax aus anderen Sprachen.  
**Lösung**: Diese Knowledge Base nutzt **RAG (Retrieval-Augmented Generation)** - Regeln werden als Kontext eingebunden, nicht ins Modell trainiert. Sofort einsatzbereit, aktualisierbar, transparent.

## 🚀 Schnellstart

1. **Ordner in Cursor öffnen** → `.cursor/rules/*.mdc` werden automatisch geladen
2. **Skripte im `workspace/` erstellen** → Cursor nutzt automatisch die Knowledge Base
3. **Fertig** → Stelle Cursor Fragen zu Ninox, Cursor folgt den Regeln

## 📁 Struktur

- `rules/` - Regeln (Function-Whitelist, Forbidden Patterns, Performance)
- `.cursor/rules/*.mdc` - Cursor Project Rules (automatisch geladen)
- `docs/` - Dokumentation (Scripting, Datenbanken, Automatisierung)
- `examples/` - Beispiele und Patterns
- `workspace/` - Hier erstellst du deine Ninox-Skripte

## 🔍 Regeln

Jedes generierte Ninox-Skript muss:

1. Funktionen aus der Whitelist verwenden (`rules/function-whitelist.md`)
2. Keine Patterns aus `rules/forbidden-patterns.md` enthalten
3. Quellenangaben zu forum.ninox.de enthalten
4. Undokumentierte Features so kennzeichnen: `⚠️ Nicht in offizieller Dokumentation, aber funktioniert`

**Prinzip**: Dokumentierte Funktionen bevorzugen. Undokumentierte aber funktionierende Features können verwendet werden - mit Kennzeichnung.

## 📖 Quellen

- Offizielle Dokumentation: https://forum.ninox.de/category/docs
- Ninox Tutorials: https://ninox.com/de/tutorials
- Community Forum: https://forum.ninox.de

## ⚖️ Lizenz

Creative Commons Attribution 4.0 International (CC-BY 4.0) - siehe [LICENSE](LICENSE).

**Hinweis**: Ninox ist eine Marke von Ninox Software GmbH. Diese Knowledge Base ist nicht offiziell von Ninox unterstützt.
