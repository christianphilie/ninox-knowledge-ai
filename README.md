# Ninox Scripting Agent Skill

Ein [Agent Skill](https://agentskills.io) für KI-Codierungs-Assistenten (**[Cursor](https://cursor.sh)**, **[Claude Desktop](https://claude.ai/download)**), der sicherstellt, dass generierte **[Ninox](https://ninox.com)**-Skripte nur dokumentierte Funktionen verwenden.

**⚠️ Problem**: KI-Modelle erfinden oft Funktionen oder übernehmen Syntax aus anderen Sprachen (wie JavaScript-Array-Methoden), die Ninox nicht unterstützt.

**✅ Lösung**: Dieser Skill folgt dem [Agent Skills Standard](https://agentskills.io/specification) und nutzt **Progressive Disclosure**. Die Regeln werden als Kontext eingebunden, sodass die KI genau weiß, was in Ninox erlaubt ist und was nicht.

## 🚀 Schnellstart

### Als Skill installieren in **Cursor** (empfohlen)

**⚠️ Wichtig**: Der Skill wird nur für das aktuelle Projekt verfügbar sein. Für beste Ergebnisse solltest du ein Ninox-Workspace-Projekt in Cursor öffnen, bevor du den Skill installierst.

1. **Cursor öffnen** und deinen Ninox-Workspace öffnen
2. **Zu "Rules, Skills, Subagents" navigieren**: `Cmd+Shift+J` (Mac) oder `Ctrl+Shift+J` (Windows/Linux)
3. **Neben "Rules" auf "New" klicken** und **"Add from GitHub" auswählen**
4. **GitHub Repository URL eingeben**: `https://github.com/christianphilie/ninox-agent-skill.git` (inkl. `.git` am Ende)
5. **Fertig**: Der Skill wird automatisch installiert und ist sofort für dieses Projekt verfügbar

Der Skill wird automatisch erkannt, wenn du Fragen zu Ninox-Skripten stellst.


## 📊 Unterstützte KI-Assistenten

| KI-Assistent | Unterstützung | Installation |
|--------------|---------------|--------------|
| **Cursor** | ✅ Vollständig | GitHub-Installation oder Repository klonen |
| **Claude Desktop** | ✅ Vollständig | Repository klonen (erkennt Skills automatisch) |
| **Alle Agent Skills-kompatiblen Tools** | ✅ Vollständig | Standard-konform |

## 📁 Projekt-Struktur

Dieses Repository enthält einen [Agent Skill](https://agentskills.io/specification) im Verzeichnis `ninox-scripting/`. Die Struktur entspricht exakt dem [Agent Skills Standard](https://agentskills.io/specification):

```
ninox-scripting/                # Skill-Verzeichnis (entspricht Agent Skills Standard)
├── SKILL.md                    # Skill-Definition mit YAML Frontmatter (erforderlich)
└── references/                 # Optionale Referenzdateien (progressive disclosure)
    ├── function-whitelist.md   # Alle dokumentierten Ninox-Funktionen
    ├── forbidden-patterns.md   # Häufige Fehler, die vermieden werden müssen
    ├── common-mistakes.md      # Best Practices und häufige Fehlerquellen
    ├── context-queries.md      # Wann nachfragen statt raten
    ├── relations-and-loops.md  # Korrekte Verwendung von Relationen und for-Schleifen
    ├── performance-rules.md    # Optimierungsrichtlinien
    ├── undocumented-features.md # Funktionierende aber undokumentierte Features
    ├── strict-rules.md         # Kritische Dokumentationskonformitäts-Regeln
    ├── style-guide.md          # Coding-Standards
    ├── functions.md            # Alle dokumentierten Ninox-Funktionen (Referenz)
    ├── performance-guide.md    # Performance-Optimierungen
    ├── api.md                  # HTTP und REST API
    ├── tables.md               # Datenbank-Tabellen-Konzepte
    └── example-*.md            # Best-Practice Beispiele und Patterns
```

**Hinweis**: Der Skill ist vollständig portabel und kann direkt von GitHub installiert werden. Alle Referenzen sind im `references/` Ordner enthalten, um progressive disclosure zu ermöglichen (Agent Skills Standard).

## ✅ Die Goldenen Regeln

Jedes von der KI generierte Ninox-Skript muss:
1. Nur dokumentierte Funktionen verwenden (siehe `references/function-whitelist.md`)
2. Keine Muster aus `references/forbidden-patterns.md` enthalten (z.B. kein `.map()`)
3. Saubere Syntax gemäß `references/style-guide.md` einhalten

## 🔧 Wie der Skill funktioniert

Der Skill nutzt **Progressive Disclosure** gemäß dem Agent Skills Standard:

1. **Discovery**: Beim Start lädt der Agent nur Name und Beschreibung des Skills
2. **Activation**: Bei relevanten Aufgaben wird die vollständige `SKILL.md` geladen
3. **Execution**: Referenzdateien aus `references/` werden bei Bedarf nachgeladen

Der Skill enthält:
- **Function Whitelist**: Alle dokumentierten Ninox-Funktionen
- **Forbidden Patterns**: Häufige Fehler, die vermieden werden müssen
- **Common Mistakes**: Best Practices und häufige Fehlerquellen
- **Performance Rules**: Optimierungsrichtlinien
- **Undocumented Features**: Funktionierende aber undokumentierte Features (mit Kennzeichnung)
- **Style Guide**: Coding-Standards
- **Context Queries**: Wann nachfragen statt raten
- **Relations and Loops**: Korrekte Verwendung von Relationen und for-Schleifen

Alle Referenzen sind im `references/` Ordner des Skills enthalten, sodass der Skill vollständig portabel ist und über GitHub installiert werden kann.

## 📚 Agent Skills Standard

Dieses Repository folgt exakt dem offenen [Agent Skills Standard](https://agentskills.io/specification), der von [Anthropic](https://www.anthropic.com/) entwickelt und als offener Standard veröffentlicht wurde. Der Standard wird von führenden AI-Entwicklungstools unterstützt, einschließlich Cursor und Claude Desktop.

**Struktur-Konformität:**
- ✅ Skill-Verzeichnis `ninox-scripting/` mit erforderlicher `SKILL.md`
- ✅ YAML Frontmatter mit `name`, `description`, `license`, `metadata`
- ✅ Optionale `references/` Verzeichnis für progressive disclosure
- ✅ Relative Pfade zu Referenzdateien in `SKILL.md`
- ✅ Skill-Dokumentation unter 500 Zeilen (Hauptanweisungen in `SKILL.md`, Details in `references/`)

## ⚖️ Lizenz

Creative Commons Attribution 4.0 International (CC-BY 4.0).

Siehe [LICENSE](LICENSE) für vollständige Lizenzbedingungen.

Ninox ist eine Marke der Ninox Software GmbH. Dieser Agent Skill ist ein Community-Projekt und wird nicht offiziell von Ninox Software GmbH unterstützt.
