---
name: ninox-scripting
description: Expert knowledge for scripting in Ninox databases, preventing hallucinations and enforcing project rules. Use when writing, debugging, or explaining Ninox scripts (NX), when the user mentions Ninox, NX scripts, database scripting, or needs help with Ninox formulas, automation, or database operations.
license: CC-BY-4.0. See LICENSE for complete terms.
metadata:
  author: christianphilie
  version: "1.3"
---

# Ninox Scripting Skill

This skill provides expert knowledge for scripting in Ninox databases, preventing hallucinations and enforcing strict rules. Use when writing, debugging, or explaining Ninox scripts (NX).

## Critical Rules

Follow these rules strictly to avoid hallucinations (inventing non-existent functions):

1. **ALWAYS** read `references/function-whitelist.md` before writing code. If a function is not in this list, **DO NOT USE IT** unless you verify it yourself in the official documentation.
2. **ALWAYS** read `references/forbidden-patterns.md` to avoid common errors (e.g., using JS array methods like `map`, `filter`, etc., which do NOT exist in Ninox).
3. **ASK FOR CONTEXT**: If field names, table names, or relations are unclear, **ASK** instead of guessing. See `references/context-queries.md`.
4. **CONSULT ALL RULES**: Review all relevant documents in the `references/` directory before writing code. See Resources section below for all available rule documents.

## Datenbankstruktur verstehen

Wenn du Ninox-Code schreiben sollst und die Tabellenstruktur unklar ist:
1. Bitte den Nutzer um den API-Export oder die manuelle Strukturbeschreibung
2. Lies `references/export-guide.md` für die Anleitung, die du dem Nutzer geben kannst
3. Frag **NIEMALS** nach Feldnamen durch Raten – das führt zu Halluzinationen

Verwende diese Prompt-Vorlage, um den Nutzer zu einer guten Beschreibung anzuleiten:
```
Damit ich den korrekten Code schreiben kann, brauche ich die genaue Datenbankstruktur.
Bitte nenne mir:
1. Den genauen Tabellennamen (mit Leerzeichen/Sonderzeichen)
2. Die Feldnamen mit Typen (Text, Zahl, Datum, Relation...)
3. Welche Tabellen zusammenhängen (Relationen)
4. Den Trigger-Kontext (wo läuft das Script? onChange auf Feld X? Button in Tabelle Y?)

Oder nutze die Anleitung in references/export-guide.md für einen API-Export.
```

## Resources

### Core Rules
* [Strict Rules](references/strict-rules.md) - Critical documentation compliance rules
* [Function Whitelist](references/function-whitelist.md) - All documented Ninox functions
* [Forbidden Patterns](references/forbidden-patterns.md) - Common errors to avoid
* [Common Mistakes](references/common-mistakes.md) - Best practices and common pitfalls
* [Context Queries](references/context-queries.md) - When to ask instead of guessing
* [Relations and Loops](references/relations-and-loops.md) - Correct usage of relations and for-loops
* [Performance Rules](references/performance-rules.md) - Optimization guidelines
* [Undocumented Features](references/undocumented-features.md) - Working but undocumented features
* [Style Guide](references/style-guide.md) - Coding standards

### Documentation
* [Functions](references/functions.md) - All documented Ninox functions
* [Performance Guide](references/performance-guide.md) - Performance optimizations
* [API](references/api.md) - HTTP and REST API (full reference)
* [HTTP Requests](references/http-requests.md) - Making HTTP requests from NX scripts
* [Tables](references/tables.md) - Database table concepts
* [Triggers](references/triggers.md) - All trigger types, contexts, and limitations
* [Datetime Functions](references/datetime-functions.md) - Date/time functions, formatting, arithmetic
* [Types](references/types.md) - Type system, conversion, null handling
* [Database Schema](references/database-schema.md) - Schema structure and field types
* [Export Guide](references/export-guide.md) - How to export database structure for AI analysis
* [Globals](references/globals.md) - Global functions: teamId, user functions, sendEmail, ninoxApp, openPage, execution blocks

* [Changelog](references/changelog.md) - Script-relevant version history and new functions by version

### Examples
* [Basic Select](references/example-basic-select.md) - Basic select statements
* [Aggregate Functions](references/example-aggregate-functions.md) - Using aggregate functions instead of loops
* [Select Optimization](references/example-select-optimization.md) - Optimizing select queries
* [Transaction Pattern](references/example-transaction-pattern.md) - Using transaction blocks
* [Count with Filter](references/example-count-filter.md) - Undocumented count() with filter pattern
* [Select with Brackets](references/example-select-brackets.md) - Undocumented bracket syntax
