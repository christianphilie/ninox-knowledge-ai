---
name: ninox-scripting
description: Expert knowledge for scripting in Ninox databases, preventing hallucinations and enforcing project rules. Use when writing, debugging, or explaining Ninox scripts (NX), when the user mentions Ninox, NX scripts, database scripting, or needs help with Ninox formulas, automation, or database operations.
license: CC-BY-4.0. See LICENSE for complete terms.
metadata:
  author: christianphilie
  version: "1.0"
---

# Ninox Scripting Skill

This skill provides expert knowledge for scripting in Ninox databases, preventing hallucinations and enforcing strict rules. Use when writing, debugging, or explaining Ninox scripts (NX).

## Critical Rules

Follow these rules strictly to avoid hallucinations (inventing non-existent functions):

1. **ALWAYS** read `references/function-whitelist.md` before writing code. If a function is not in this list, **DO NOT USE IT** unless you verify it yourself in the official documentation.
2. **ALWAYS** read `references/forbidden-patterns.md` to avoid common errors (e.g., using JS array methods like `map`, `filter`, etc., which do NOT exist in Ninox).
3. **ASK FOR CONTEXT**: If field names, table names, or relations are unclear, **ASK** instead of guessing. See `references/context-queries.md`.
4. **CONSULT ALL RULES**: Review all relevant documents in the `references/` directory before writing code. See Resources section below for all available rule documents.

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
* [API](references/api.md) - HTTP and REST API
* [Tables](references/tables.md) - Database table concepts

### Examples
* [Basic Select](references/example-basic-select.md) - Basic select statements
* [Aggregate Functions](references/example-aggregate-functions.md) - Using aggregate functions instead of loops
* [Select Optimization](references/example-select-optimization.md) - Optimizing select queries
* [Transaction Pattern](references/example-transaction-pattern.md) - Using transaction blocks
* [Count with Filter](references/example-count-filter.md) - Undocumented count() with filter pattern
* [Select with Brackets](references/example-select-brackets.md) - Undocumented bracket syntax
