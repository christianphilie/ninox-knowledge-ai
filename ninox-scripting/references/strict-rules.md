# Strict Rules for Ninox Scripting

## CRITICAL: Documentation Compliance with Nuances

**MOST IMPORTANT RULE**: Ninox script is very limited. There are three categories of functions and features:

### 1. Documented Functions ✅
**Status**: Prefer to use, standard approach

- All functions that are in the official Ninox documentation
- Functions from official tutorials (ninox.com/tutorials)
- Functions described in the official documentation on forum.ninox.de
- Source citation required: Always provide URL to documentation

**Examples**:
- `select ... where ...` - Documented in Ninox Tutorials
- `sum()`, `count()`, `first()`, `last()` - Documented aggregate functions
- `if ... then ... else` - Documented conditional logic
- `do as transaction ... end` - Documented performance optimization

### 2. Undocumented but Working ⚠️
**Status**: Can be used, but with appropriate labeling/warning

- Features that are not in the official documentation but proven to work
- Must be confirmed by at least two independent sources or official community examples
- **MANDATORY LABELING**: Always mark with comment: `⚠️ Not in official documentation, but works`
- When using: Explicitly indicate that it is not documented

**Examples**:
- `select Table[ Condition ]` - Square brackets as filter (works, but performance difference from `where`)
- Combinations of features that are not explicitly documented but work

### 3. Non-existent Functions ❌
**Status**: ABSOLUTELY FORBIDDEN

- Do not invent anything or adopt from other programming languages
- Do not use functions that do not exist
- Do not use syntax that is not supported in Ninox
- If uncertain: Always use documented alternative or explicitly warn

**Common mistakes**:
- Adopting functions from JavaScript/Python/etc. (e.g., `Array.map()`, `String.split()`)
- Invented functions (e.g., `find()`, `filter()` as separate functions)
- Syntax from other languages (e.g., `for ... in ...`, `try ... catch`)

## Principle

**Prefer documented solutions**, but if something works (even if not documented), it can be used - **with appropriate labeling**.

## Validation Process

Before every generated Ninox script:

1. ✅ Check Function Whitelist - prefer documented functions
2. ⚠️ Check undocumented-features.md - if used, label appropriately
3. ❌ Check forbidden-patterns.md - no invented functions/syntax
4. 📚 Add source citations for documented features
5. ⚠️ Explicitly label undocumented features

## When Uncertain

If you are not sure whether a function exists:

1. **First**: Look in function-whitelist.md
2. **Then**: Look in undocumented-features.md
3. **Then**: Look in forbidden-patterns.md
4. **If not found**: Mark as "not confirmed" and use documented alternative
5. **Never**: Invent anything or adopt from other languages

## Sources

- Official Documentation: https://forum.ninox.de/category/docs
- Ninox Tutorials: https://ninox.com/de/tutorials
- Community Forum: https://forum.ninox.de
