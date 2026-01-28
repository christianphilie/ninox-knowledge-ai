# Ninox AI Knowledge Base

Eine universelle Knowledge Base für KI-Codierungs-Assistenten (**[Cursor](https://cursor.sh)**, **[Google Antigravity](https://antigravity.dev)**, etc.), die mittels RAG sicherstellt, dass generierte **[Ninox](https://ninox.com)**-Skripte nur dokumentierte Funktionen verwenden.

**⚠️ Problem**: KI-Modelle erfinden oft Funktionen oder übernehmen Syntax aus anderen Sprachen (wie JavaScript-Array-Methoden), die Ninox nicht unterstützt.

**✅ Lösung**: Diese Knowledge Base nutzt **RAG (Retrieval-Augmented Generation)**. Die Regeln werden als Kontext eingebunden, sodass die KI genau weiß, was in Ninox erlaubt ist und was nicht.


## 🚀 Schnellstart

1.  **Repository klonen** oder herunterladen.
2.  **In Cursor oder Google Antigravity öffnen**: 
    - **Cursor**: Regeln in `.cursor/rules/` und Skills in `.cursor/skills/` werden automatisch erkannt
    - **Google Antigravity**: Skills in `.agent/skills/` werden automatisch erkannt
3.  **Skripte erstellen**: Erstelle deine `.ninox` Dateien im Ordner `workspace/`.

## 📊 Vergleich der KI-Assistenten

| KI-Assistent | Bewertung | Kosten | Regelbefolgung | Empfehlung |
|--------------|-----------|--------|----------------|------------|
| **Cursor** | ⭐⭐⭐⭐⭐ | Kostenpflichtig | Sehr gut | ✅ **Empfohlen** |
| **Google Antigravity** | ⭐⭐⭐⭐ | Teilweise kostenlos | Gut | ✅ Gute Alternative |
| **ChatGPT Custom GPT** | ⭐⭐⭐ | Kostenpflichtig | Schwächer | ⚠️ Funktioniert, aber weniger zuverlässig |

## 📖 Detaillierte Einrichtungsanleitung

### 1. Vorbereitung: Terminal öffnen und Ordner wählen

1.  **Terminal öffnen** (macOS: Terminal.app, Windows: PowerShell oder Git Bash, Linux: Terminal)
2.  **Zum gewünschten Ordner navigieren**, in dem du das Projekt speichern möchtest:
    ```bash
    cd ~/Sites
    # oder
    cd ~/Documents
    ```

### 2. Installation: Repository klonen

Klone das Repository in den aktuellen Ordner und wechsle danach in den neuen Ordner:
```bash
git clone https://github.com/christianphilie/ninox-knowledge-ai.git
cd ninox-knowledge-ai
```

### 3. Projekt in deinem KI-Assistenten öffnen

#### Für Cursor

1.  **Cursor öffnen** (falls noch nicht installiert: von [cursor.sh](https://cursor.sh) herunterladen)
2.  **Projekt öffnen**: `File` → `Open Folder` → Wähle den `ninox-knowledge-ai` Ordner
3.  **Fertig**: 
    - Cursor liest automatisch alle `.mdc` Dateien im Ordner `.cursor/rules/`. Die "Leitplanken" sind sofort aktiv.
    - Das Ninox-Scripting Skill in `.cursor/skills/` ist verfügbar und kann mit `@ninox-scripting` aufgerufen werden.
4.  **KI-Assistenten nutzen**: Öffne die Chat-Funktion in Cursor (Cmd/Ctrl + L) und stelle Fragen zu Ninox-Skripten.

#### Für Google Antigravity

1.  **Google Antigravity öffnen** (falls noch nicht installiert: von [antigravity.dev](https://antigravity.dev) herunterladen)
2.  **Projekt öffnen**: `File` → `Open Folder` → Wähle den `ninox-knowledge-ai` Ordner
3.  **Fertig**: Antigravity erkennt automatisch den `.agent/`-Ordner. Die Skills werden automatisch geladen.
4.  **KI-Assistenten nutzen**: Die Ninox-Scripting Skills sind automatisch verfügbar, wenn du Fragen zu Ninox stellst.

## 🤖 Konfiguration für ChatGPT (Custom GPT)

ChatGPT Custom GPTs funktionieren anders als Cursor oder Antigravity - sie müssen direkt in ChatGPT eingerichtet werden. Eine detaillierte Schritt-für-Schritt-Anleitung findest du in [`.custom-gpt/README.md`](.custom-gpt/README.md).


## 📁 Projekt-Struktur

- `rules/` - Die Kern-Regeln (Whitelist, Verbotene Patterns, Performance)
- `.cursor/rules/` - Spezielle Konfiguration für den Cursor Editor (Rules)
- `.cursor/skills/` - Cursor Skills (z.B. `@ninox-scripting` für Ninox-Expertise)
- `.agent/` - Spezielle Konfiguration für Google Antigravity (Skills & Workflows)
- `.custom-gpt/` - Anleitungen und Texte für ChatGPT (Custom GPT)
- `docs/` - Tiefergehende Dokumentation zu Scripting und Automatisierung
- `examples/` - Best-Practice Beispiele für Ninox-Skripte
- `workspace/` - Dein Arbeitsbereich für neue Skripte


## ✅ Die Goldenen Regeln

Jedes von der KI generierte Ninox-Skript muss:
1.  Nur Funktionen aus der `rules/function-whitelist.md` verwenden.
2.  Keine Muster aus `rules/forbidden-patterns.md` enthalten (z.B. kein `.map()`).
3.  Saubere Syntax gemäß `rules/style-guide.md` einhalten.


## ⚖️ Lizenz
Creative Commons Attribution 4.0 International (CC-BY 4.0).
Ninox ist eine Marke der Ninox Software GmbH. Diese Knowledge Base ist ein Community-Projekt.

