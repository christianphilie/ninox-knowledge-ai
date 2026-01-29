# Ninox Scripting - Style Guide

## Overview

This style guide describes coding standards and best practices for Ninox scripts.

---

## Code Formatting

### Indentation
Use consistent indentation (e.g., 2 spaces or tabs).

### Line Length
Keep lines readable. Break long lines.

### Blank Lines
Use blank lines to group logically related code blocks.

---

## Variable Names

### Naming
- Use meaningful names
- Use camelCase for variables
- Use clear, descriptive names

### Examples
```ninox
"✅ GOOD";
let customerName := "Max Mustermann";
let orderTotal := sum(select Orders[Amount]);

"❌ BAD";
let x := "Max Mustermann";
let t := sum(select Orders[Amount]);
```

### this Context: Always Store in Variable
**IMPORTANT**: To avoid this context problems, store `this` at the beginning in a variable:

```ninox
"Best Practice: Store this in variable";
let my := this;
"Then use 'my' everywhere instead of 'this'";
for item in my.Relation do
  "Processing";
end
```

**Reason**: `this` can cause problems in certain contexts (loops, nested functions). `let my := this;` reliably stores the context.

See also: `references/common-mistakes.md` - Mistake 13

---

## Comments

### IMPORTANT: Comment Syntax in Ninox

**In Ninox, comments do NOT work with `//` or `/* */`!**

Comments are written as string statements:

```ninox
let code := 1;
"This is a comment!";
let moreCode := 2;
"Multi-line comments";
"must be written as separate";
"string statements";
```

### When to Comment
- Explain complex logic
- Label undocumented features
- Document important decisions

### Format
```ninox
"Single-line comment";

"Multi-line comments";
"must be written as separate";
"string statements";

"⚠️ Not in official documentation, but works";
let records := select Table[Condition];
```

### Example
```ninox
let my := this;
"Calculates stock from warehouse bookings";
"⚠️ Not in official documentation, but works: Relation[Filter]";
first(my.Product.WarehouseBookings[Warehouse = my.'From Source Warehouse']).'Stock in Base Unit in This Warehouse'
```

---

## Functions

### Custom Functions
- Use meaningful function names
- Document parameters and return values
- Keep functions focused

### Example
```ninox
"Calculates total price including tax";
function calculateTotal(amount, taxRate) do
  amount + (amount * taxRate)
end
```

---

## Error Handling

### Validation
Validate inputs before using them.

```ninox
if amount > 0 then
  "Processing";
else
  "Error handling";
end
```

---

## Performance

### Best Practices
- Use `select ... where` instead of subsequent filtering
- Use `do as transaction` for multiple write operations
- Avoid `select` in loops
- Use aggregate functions instead of loops

See also: `references/performance-rules.md`

---

## Documentation Compliance

### Rules
1. Prefer documented functions
2. Label undocumented features
3. Do not use invented functions

---

## Examples

### Good Code Style
```ninox
"Calculates total of all open orders";
function calculateOpenOrdersTotal() do
  let openOrders := select Orders where Status = "Open";
  sum(openOrders[Amount])
end

"Main logic";
do as transaction
  let total := calculateOpenOrdersTotal();
  if total > 1000 then
    "Processing";
  end
end
```

### Bad Code Style
```ninox
"No comments, bad names, no performance optimization";
let o := select Orders;
let t := 0;
for x in o do
  if x.Status = "Open" then
    t := t + x.Amount;
  end
end
```

---

## Checklist

Before every script check:

- [ ] Meaningful variable names
- [ ] Comments for complex logic
- [ ] Performance optimizations considered
- [ ] Documentation compliance checked
- [ ] Code formatted and readable

---

**Sources**:
- Official Documentation: https://forum.ninox.de/category/docs
- Best Practices: https://forum.ninox.de
