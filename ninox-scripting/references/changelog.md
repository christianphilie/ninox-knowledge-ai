# Ninox Scripting — Changelog / Version History

## Overview

This document tracks script-relevant changes across Ninox versions. Focus: new functions, changed behavior, bug fixes for scripting.

**Note**: Only script/developer-relevant changes are listed here. For full release notes, see https://forum.ninox.com/t/g9yzkrh/release-notes

---

## Ninox 3.18.x (Feb–Mar 2026)

### 3.18.5 (Mar 5, 2026)
- Bug fix: Non-admin users logged out when using icon selector (not script-related)

### 3.18.2 (Feb 18, 2026)
- **Bug fix**: `get()` function with a `null` argument caused runtime crash/hang in browser clients — now handles null safely
- **Bug fix**: Global functions with date parameters failed when executed across modules — now works seamlessly across modules

### 3.18.1 (Feb 12, 2026)
- **Feature**: Modularity now supports formula fields, dynamic choice fields, and calculated fields across modules
- **Feature**: Public global functions no longer require unnecessary table creation

### 3.18 (Feb 4, 2026)
- **Feature**: Modularity open beta — shared global functions across modules
- **Feature**: Native plugin integration

---

## Ninox 3.17.x (Jul 2025 – Jan 2026)

### 3.17.19 (Jan 15, 2026)
- **Bug fix**: Saving a global function failed if it contained a local variable assigned to a JSON value
- **Bug fix**: `http()` function timed out unexpectedly after 3 seconds due to a global axios timeout limitation — now resolved

### 3.17.18 (Jan 14, 2026)
- **Bug fix**: `createZipFile()` function failed on Windows in v3.17.17 — now fixed

### 3.17.17 (Jan 13, 2026)
- **Bug fix**: External API calls returned "Not authorized" errors after updating from 3.17.14 to 3.17.15 — resolved

### 3.17 (Jul 28, 2025)
- No script-specific features noted (major feature: Ninox HR Module included in all paid plans)

---

## Ninox 3.15 (Feb 18, 2025)

- **Feature**: Hide Record Controls — developers can hide UI controls (print, create, delete) per table/view
- **Feature**: Password-Protected Modules — secure cross-module connections

---

## Ninox 3.14 (Nov 19, 2024)

- **NEW FUNCTION**: `parseCSV(text)` — parses CSV-formatted text into structured arrays/lists for use in scripts
- **Feature**: JSON values now supported in dynamic choice fields (can assign JSON arrays programmatically)
- **Feature**: `printAndSaveRecord()` updated to exclusively support Carbone reports; new `_options` parameter supports watermarking, password protection, PDF versioning

---

## Ninox 3.13 (Aug 2024)

- **Major UI redesign**: New navigation concept (tabs replaced with Pages feature)
- No scripting-breaking changes reported, but `openPage()` became more relevant as main navigation method

---

## Ninox 3.11 (Jan 30, 2024)

- **Feature**: "Creatable If" & "Deletable If" conditional logic — control when records can be created or deleted
- **Feature**: Automated database reconnection for cross-workspace relations

---

## Ninox 3.10 (Oct 2023)

- **NEW FEATURE**: Pages — create full-page application interfaces and dashboards
- `openPage()` function introduced as part of this feature
- **Feature**: Modularity — link tables between databases in the same workspace
- **Feature**: Mail Hook — receive emails directly to a Ninox mailbox

---

## Functions Added Per Version (Summary Table)

| Function | Version Introduced | Notes |
|---|---|---|
| `openPage()` | ~3.10 | Added with Pages feature |
| `parseCSV()` | 3.14 | CSV import into scripts |
| `createZipFile()` | Unknown | Pre-3.17 |
| `printAndSaveRecord()` | Unknown | Updated significantly in 3.14 |
| `get()` | Unknown | Null safety improved in 3.18.2 |

---

## Deprecated / Changed Behaviors

| Item | Change | Version |
|---|---|---|
| `http()` timeout | Was limited to 3 seconds due to axios bug — fixed | 3.17.19 |
| `printAndSaveRecord()` | Now exclusively uses Carbone; old template formats may not work | 3.14 |
| `createZipFile()` | Windows incompatibility fixed | 3.17.18 |

---

## Sources

- Ninox Release Notes (community forum): https://forum.ninox.com/t/g9yzkrh/release-notes
- Ninox Blog: https://ninox.com/en/blog/whats-new-at-ninox-fresh-release-information
- Official Docs: https://docs.ninox.com/en/resources/product-updates
