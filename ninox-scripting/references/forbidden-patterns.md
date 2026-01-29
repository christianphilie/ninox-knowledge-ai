# Forbidden Patterns - Common LLM Mistakes

This list contains functions, syntax, and patterns that **DO NOT** exist in Ninox but are frequently invented by LLMs.

## ❌ ABSOLUTELY FORBIDDEN

### JavaScript/Python Functions That Do Not Exist

#### Array Methods
- ❌ `array.map()` - Does not exist
- ❌ `array.filter()` - Does not exist (use `select ... where` or `[]` filter)
- ❌ `array.forEach()` - Does not exist (use `for ... in ... do`)
- ❌ `array.reduce()` - Does not exist
- ❌ `array.find()` - Does not exist (use `select ... where` with `first()`)
- ❌ `array.includes()` - Does not exist
- ❌ `array.indexOf()` - Does not exist

**Correct Alternative**: Use `select ... where` or loops with `for ... in ... do`

#### String Methods
- ❌ `string.split()` - Does not exist
- ❌ `string.join()` - Does not exist
- ❌ `string.replace()` - Does not exist
- ❌ `string.substring()` - Does not exist
- ❌ `string.trim()` - Does not exist

**Correct Alternative**: Use documented string functions (see function-whitelist.md)

#### Object/Record Methods
- ❌ `Object.keys()` - Does not exist
- ❌ `Object.values()` - Does not exist
- ❌ `Object.entries()` - Does not exist

**Correct Alternative**: Direct field access via dot notation

### Syntax from Other Languages

#### Try-Catch
- ❌ `try ... catch ... finally` - Does not exist in Ninox

**Correct Alternative**: Error handling via `if` conditions and validation

#### For Loops
- ❌ `for (let i = 0; i < length; i++)` - JavaScript syntax, does not exist
- ❌ `for item in array:` - Python syntax, does not exist

**Correct Alternative**: `for ... in ... do` (Ninox syntax)

#### Arrow Functions
- ❌ `() => {}` - JavaScript syntax, does not exist

**Correct Alternative**: Functions with `function name() do ... end`

#### Template Literals
- ❌ `` `String ${variable}` `` - JavaScript syntax, does not exist

**Correct Alternative**: String concatenation with `+` or `concat()`

### Invented Ninox Functions

- ❌ `find()` - Does not exist (use `select ... where` with `first()`)
- ❌ `search()` - Does not exist
- ❌ `filter()` - Does not exist (use `select ... where`)
- ❌ `sort()` - Does not exist directly (use `select ... order by`)
- ❌ `unique()` - Does not exist
- ❌ `groupBy()` - Does not exist directly

**Correct Alternative**: Use `select` with corresponding clauses

### Regular Expressions

- ❌ Regular expressions like `/pattern/` - Not directly supported
- ❌ `match()`, `test()` - Do not exist

**Correct Alternative**: Use string functions like `contains()`, `startsWith()`, `endsWith()`

### JSON Manipulation

- ❌ `JSON.parse()` - Does not exist directly
- ❌ `JSON.stringify()` - Does not exist directly

**Correct Alternative**: Ninox has its own data types, direct field access

### Asynchronous Functions

- ❌ `async/await` - Does not exist
- ❌ `Promise` - Does not exist
- ❌ `.then()`, `.catch()` - Do not exist

**Correct Alternative**: Ninox scripts run synchronously

## Common Error Sources

### 1. Assumption of JavaScript Similarity
**Problem**: LLMs assume Ninox works like JavaScript
**Solution**: Always check function-whitelist.md

### 2. Assumption of SQL Similarity
**Problem**: LLMs use SQL syntax that does not exist
**Solution**: Ninox has its own syntax, see documentation

### 3. Inventing "Practical" Functions
**Problem**: LLMs invent functions that seem "useful"
**Solution**: Only use documented functions

## Validation

Before every script check:
1. Does the script contain functions from this list? → REMOVE
2. Is there a documented alternative? → USE
3. Is something uncertain? → Mark as "not confirmed"

## Sources

- Official Documentation: https://forum.ninox.de/category/docs
- Function Whitelist: `references/function-whitelist.md`
