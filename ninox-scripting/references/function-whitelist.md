# Function Whitelist - Documented Ninox Functions

This list contains all **documented** Ninox functions that can be safely used. These functions should be preferred.

## Sources

- Official Documentation: https://forum.ninox.de/category/docs
- Ninox Tutorials: https://ninox.com/de/tutorials
- Community Forum: https://forum.ninox.de

---

## Basic Syntax

### Variables and Assignments
- `let variable := value` - Define variable
- `variable := value` - Assign variable

**Source**: https://ninox.com/de/tutorials/basic-scripting

### Conditional Logic
- `if condition then ... else ... end` - Conditional execution
- `switch value do case ... end` - Multiple selection

**Source**: https://ninox.com/de/tutorials/if-then-else-statement

### Loops
- `while condition do ... end` - While loop
- `for item in collection do ... end` - For loop over collection
- `repeat ... until condition` - Repeat-until loop

**Source**: Ninox Tutorials

---

## Database Operations

### Select Statements
- `select Table` - All records from table
- `select Table where condition` - Filtered records
- `select Table order by field` - Sorted records
- `select Table limit number` - Limited count

**Source**: https://ninox.com/de/tutorials/advanced-tutorial-2

### Record Operations
- `create Table` - Create new record
- `record.field := value` - Set field
- `record.field` - Read field
- `delete record` - Delete record

**Source**: Ninox Documentation

---

## Aggregate Functions

### Mathematical Functions
- `sum(collection)` - Calculate sum
- `count(collection)` - Count items
- `average(collection)` - Calculate average
- `min(collection)` - Find minimum
- `max(collection)` - Find maximum

**Source**: Ninox Documentation

### Record Functions
- `first(collection)` - First record
- `last(collection)` - Last record
- `item(collection, index)` - Element at index position

**Source**: Ninox Documentation

---

## String Functions

- `concat(string1, string2, ...)` - Concatenate strings
- `contains(text, search)` - Contains text
- `startsWith(text, prefix)` - Starts with
- `endsWith(text, suffix)` - Ends with
- `length(text)` - Text length
- `upper(text)` - Uppercase
- `lower(text)` - Lowercase
- `trim(text)` - Remove whitespace (if documented)
- `split(text, delimiter)` - Split text at delimiter
- `substr(text, start, length)` - Extract substring
- `index(text, search)` - Find position of text
- `replace(text, search, replacement)` - Replace text
- `join(collection, separator)` - Join collection with separator

**Source**: Ninox Documentation - Functions

---

## Date and Time Functions

- `today()` - Today's date
- `now()` - Current timestamp
- `date(year, month, day)` - Create date
- `time(hour, minute, second)` - Create time
- `datetime(date, time)` - Combine date and time
- `year(date)` - Extract year
- `month(date)` - Extract month
- `day(date)` - Extract day
- `weekday(date)` - Day of week
- `week(date)` - Calendar week
- `quarter(date)` - Quarter
- `format(date/datetime, formatString)` - Format date/time

**Source**: Ninox Documentation - Functions

---

## Mathematical Functions

- `abs(number)` - Absolute value
- `round(number, decimals)` - Round (with optional decimal places)
- `floor(number)` - Round down
- `ceil(number)` - Round up
- `sqrt(number)` - Square root
- `pow(base, exponent)` - Power
- `max(value1, value2, ...)` - Maximum
- `min(value1, value2, ...)` - Minimum

**Source**: Ninox Documentation - Functions

---

## Logical Functions

- `and(condition1, condition2)` - Logical AND
- `or(condition1, condition2)` - Logical OR
- `not(condition)` - Logical negation

**Source**: Ninox Documentation

---

## Performance Optimizations

### Transaction Blocks
- `do as transaction ... end` - Transaction block for multiple write operations

**Source**: https://forum.ninox.de (Performance Documentation)

### Server Execution
- `do as server ... end` - Execution on server side

**Source**: Ninox Documentation

### Deferred Execution
- `do as deferred ... end` - Deferred execution

**Source**: Ninox Documentation

---

## Custom Functions

### Define Function
- `function name(params) do ... end` - Create custom function
- `function name(params: type): returnType do ... end` - With types

**Parameter Types**:
- `text`
- `number`
- `boolean`
- `date`
- `time`
- `datetime`
- Table types (e.g., `'Table'`)

**Source**: https://forum.ninox.com/t/x2yz1t2/create-your-own-functions

## Additional Functions

- `unique(collection)` - Remove duplicates
- `array(...)` - Create array
- `text(value)` - Convert value to text
- `number(value)` - Convert value to number
- `boolean(value)` - Convert value to boolean

**Source**: Ninox Documentation

---

## HTTP Operations

- `http(method, url, headers, body)` - Execute HTTP request
  - Method: "GET", "POST", "PUT", "DELETE"
  - Headers: Object with header key-value pairs
  - Body: Request body (for POST/PUT)

**Source**: Ninox API Documentation

---

## Field Access

- `record.field` - Read field
- `record.field := value` - Set field
- `record.relationField` - Relation field
- `record.relationField.field` - Field in relation

**Source**: Ninox Documentation

---

## Data Types

- `text` - Text
- `number` - Number
- `boolean` - Boolean value
- `date` - Date
- `time` - Time
- `datetime` - Date and time
- Table types (References)

**Source**: Ninox Documentation

---

## Important Notes

1. **This list is not complete** - It is continuously expanded
2. **Always cite source** - When using a function, provide the source
3. **When uncertain** - Look up in official documentation
4. **Prefer to use** - These functions are documented and safe

## Extension

If you find a new documented function:
1. Add it to this list
2. Provide the source (URL to documentation)
3. Add a short example

---

**Last Updated**: Initial creation based on official documentation
