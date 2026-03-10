# Common Mistakes in Ninox Scripts

## Overview

This list describes common mistakes and how to avoid them.

---

## Mistake 1: Select without Where

### ❌ WRONG
```ninox
let allOrders := select Orders;
let openOrders := allOrders[Status = "Open"];
```

### ✅ CORRECT
```ninox
let openOrders := select Orders where Status = "Open";
```

**Reason**: `where` filters during selection, which means better performance.

---

## Mistake 2: Select in Loop

### ❌ WRONG
```ninox
for customer in select Customers do
  let orders := select Orders where Customer = customer;
  "Processing";
end
```

### ✅ CORRECT
```ninox
let allOrders := select Orders;
for customer in select Customers do
  let orders := allOrders[Customer = customer];
  "Processing";
end
```

**Reason**: Each `select` is a database access. Avoid multiple accesses.

---

## Mistake 3: No Transaction for Multiple Write Operations

### ❌ WRONG
```ninox
record1.field := value1;
record2.field := value2;
record3.field := value3;
```

### ✅ CORRECT
```ninox
do as transaction
  record1.field := value1;
  record2.field := value2;
  record3.field := value3;
end
```

**Reason**: Transaction blocks reduce the number of database accesses.

---

## Mistake 4: Loop Instead of Aggregate Function

### ❌ WRONG
```ninox
let total := 0;
for order in select Orders do
  total := total + order.Amount;
end
```

### ✅ CORRECT
```ninox
let total := sum(select Orders[Amount]);
```

**Reason**: Aggregate functions are optimized and faster.

---

## Mistake 5: Using Invented Functions

### ❌ WRONG
```ninox
"These functions do NOT exist in Ninox";
let filtered := orders.filter(o => o.Status = "Open");
let mapped := orders.map(o => o.Amount);
let joined := array.join(orders, ",");
```

### ✅ CORRECT
```ninox
"Use documented functions";
let filtered := select Orders where Status = "Open";
let amounts := filtered[Amount];
let joined := concat(amounts);
```

**Reason**: Only use documented functions. See `references/forbidden-patterns.md`

---

## Mistake 6: Wrong Syntax from Other Languages

### ❌ WRONG
```ninox
"JavaScript syntax - does NOT work";
for (let i = 0; i < length; i++) {
  "Code";
}

"Python syntax - does NOT work";
for item in array:
  "Code";
```

### ✅ CORRECT
```ninox
"Ninox syntax";
for item in collection do
  "Code";
end
```

**Reason**: Ninox has its own syntax. See `references/forbidden-patterns.md`

---

## Mistake 7: Undocumented Features Without Labeling

### ❌ WRONG
```ninox
"Undocumented feature without labeling";
let records := select Table[Condition];
```

### ✅ CORRECT
```ninox
"⚠️ Not in official documentation, but works";
let records := select Table[Condition];
```

**Reason**: Undocumented features must be labeled. See `references/undocumented-features.md`

---

## Mistake 8: No Validation

### ❌ WRONG
```ninox
let result := amount / divisor;
"Can cause division by zero";
```

### ✅ CORRECT
```ninox
if divisor != 0 then
  let result := amount / divisor;
else
  "Error handling";
end
```

**Reason**: Always validate inputs.

---

## Mistake 9: Loading All Records Instead of Limit

### ❌ WRONG
```ninox
let recentOrders := select Orders order by Date desc;
```

### ✅ CORRECT
```ninox
let recentOrders := select Orders order by Date desc limit 10;
```

**Reason**: For large datasets, only load needed data.

---

## Mistake 10: Nested Selects

### ❌ WRONG
```ninox
let customers := select Customers;
for customer in customers do
  let orders := select Orders where Customer = customer;
  for order in orders do
    let items := select OrderItems where Order = order;
  end
end
```

### ✅ CORRECT
```ninox
let allOrders := select Orders;
let allItems := select OrderItems;
for customer in select Customers do
  let orders := allOrders[Customer = customer];
  for order in orders do
    let items := allItems[Order = order];
  end
end
```

**Reason**: Avoid nested selects for better performance.

---

## Mistake 11: Where Clause in for Loop with Relations

### ❌ WRONG
```ninox
"Does NOT work: where clause cannot be used directly in for loop";
for i in select 'Invoice Positions' where Invoices = myID order by Pos do
  "Code";
end
```

### ✅ CORRECT
```ninox
"With relations, iterate directly over the relation";
"'Invoice Positions' is already filtered by context (this)";
for i in 'Invoice Positions' do
  "Code";
end

"Or: First select, then iterate";
let positions := select 'Invoice Positions' where Invoices = myID order by Pos;
for i in positions do
  "Code";
end
```

**Reason**: `for` loops cannot be directly combined with `select ... where`. With relations (like `'Invoice Positions'`), iterate directly over the relation, which is already filtered by context.

**Important**: 
- Relations are already context-filtered (e.g., `this.'Invoice Positions'` shows only positions of the current invoice)
- With `select` statements, filtering must be explicit
- `order by` can only be used in `select` statements, not directly in `for` loops

---

## Mistake 12: Wrong Field Names Through Assumptions

### ❌ WRONG
```ninox
"Assumption: Field is called 'Pos', but actually it's 'Pos Nr'";
for i in 'Invoice Positions' do
  newPos.Pos := i.Pos;
  "Does not work!";
end

"Assumption: Field is called 'Unit', but actually it's 'Einheit'";
newPos.Unit := i.Unit;
"Does not work!";
```

### ✅ CORRECT
```ninox
"First ask or check what the fields are exactly called";
"Then use correct field names";
for i in 'Invoice Positions' do
  newPos.'Pos Nr' := i.'Pos Nr';
  "Correct field name";
  newPos.Einheit := i.Einheit;
  "Correct field name";
end
```

**Reason**: Field names can have various variants. Common variants:
- Position numbers: `Pos`, `Pos Nr`, `Position`, `PosNr`
- Units: `Einheit`, `Mengeneinheit`, `Mengeneinheit_alt`, `Unit`
- Date fields: `Datum`, `Erstellungsdatum`, `'Invoice Date'`, `'Billing Date From'`

**Important**: If field names are not unambiguous, ALWAYS ask instead of guessing! See `references/context-queries.md`

---

## Mistake 13: Avoiding this Context Problems

### ❌ PROBLEMATIC
```ninox
"this can cause problems in certain contexts";
for booking in this.Product.WarehouseBookings do
  if booking.Warehouse = this.'From Source Warehouse' then
    "Processing";
  end
end
```

**Problem**: `this` can reference the wrong context or show unexpected behavior in certain contexts (e.g., in loops, nested functions, or more complex operations).

### ✅ CORRECT: Store this in Variable
```ninox
"Store this in variable to avoid context problems";
let my := this;
for booking in my.Product.WarehouseBookings do
  if booking.Warehouse = my.'From Source Warehouse' then
    "Processing";
  end
end
```

**Reason**: 
- `let my := this;` reliably stores the current context
- `my` retains the context even in nested structures
- Avoids problems with `this` in loops or more complex operations
- Standard pattern for reliable context access

**When to use**:
- Always when `this` is used multiple times
- In loops that access `this`
- With nested operations
- In more complex scripts to avoid context problems

**Best Practice**: 
- At the beginning of the script: `let my := this;`
- Then use `my` everywhere instead of `this`
- Especially important with relations: `my.Relation` instead of `this.Relation`

---

## Mistake 14: Using Relation Fields Directly in select

### ❌ WRONG
```ninox
"Relation field cannot be used directly in select";
let articlePositions := select 'Pos Products /Services' where ServiceReport = thisServiceReport and 'Product Type' = 2;
```

**Problem**: `'Pos Products /Services'` is a relation field (relation), not a table. Relation fields cannot be used directly in `select` statements.

### ✅ CORRECT: Use Table
```ninox
"Use the underlying table instead of the relation field";
let articlePositions := select 'Pos Products' where ServiceReport = thisServiceReport and 'Product Type' = 2;
```

**Reason**: 
- Relation fields are only references to records in other tables
- `select` requires the name of the actual table
- The table contains the records, the relation field only points to them

**Important**: 
- Relation fields can be used directly in `for` loops: `for pos in 'Pos Products /Services' do`
- For `select` with `where`, the table must be used: `select 'Pos Products' where ...`
- See also `references/relations-and-loops.md` for details on relations

---

## Mistake 15: Setting Dynamic Selection Fields with Record Object

### ❌ WRONG
```ninox
"Error: Field must return dynamic values";
let warehouseLocation := first(select WarehouseLocations where WarehouseManagement = warehouseManagement and User = employeeUser);
newWithdrawal.(
    Warehouse := warehouseLocation;  "Does not work with dynamic selection fields!"
)
```

**Problem**: Dynamic selection fields expect a collection or a numeric value, not directly a record object. The error "Field must return dynamic values" occurs.

### ✅ CORRECT: Use number()
```ninox
"Use number() to convert record to numeric value";
let warehouseLocation := first(select WarehouseLocations where WarehouseManagement = warehouseManagement and User = employeeUser);
newWithdrawal.(
    Warehouse := number(warehouseLocation);  "Converts record to numeric value"
)
```

**Reason**: 
- Dynamic selection fields are based on table records
- `number(record)` converts the record to a numeric value (ID/No)
- This numeric value can then be used in the dynamic selection field

**Alternative Solutions** (if `number()` doesn't work):
- Collection with one element: `Warehouse := select WarehouseLocations where No = warehouseLocation.No;`
- Direct select in assignment block: `Warehouse := first(select WarehouseLocations where ...);`

**Note**: 
- Only works if the dynamic selection field is based on the same table
- With relation fields, the record can be used directly: `RelationField := record;`

---

## Mistake 16: No Check Against Duplicate Execution

### ❌ WRONG
```ninox
"Operation is executed on every call, even if already done";
"Create warehouse bookings for used articles";
let warehouseManagement := first(select WarehouseManagement);
"Creates warehouse bookings, even if already present";
```

**Problem**: If a script is executed multiple times (e.g., by clicking a button again or through error handling), operations are executed multiple times, leading to duplicates.

### ✅ CORRECT: Use Flag Field
```ninox
"Check with flag field prevents duplicate execution";
if not 'Warehouse Bookings for Used Articles Already Created' then
    "Create warehouse bookings for used articles";
    let warehouseManagement := first(select WarehouseManagement);
    "Creates warehouse bookings only once";
    "At the end:";
    'Warehouse Bookings for Used Articles Already Created' := true;
end
```

**Reason**: 
- Flag field (Boolean) marks whether operation has already been executed
- Check at the beginning prevents re-execution
- Flag is set at the end, after successful execution

**Best Practices**:
- Flag field should have a descriptive name
- Flag should only be set at the end, after successful execution
- On errors, flag should not be set, so retry is possible
- Alternative: Check for already existing records instead of flag field

---

## Mistake 17: Date Comparison with Strings

### ❌ WRONG
```ninox
"Dates are NOT strings — comparing with string does not work"
if Deadline < "2024-12-31" then
    "Error or wrong result!";
end
```

### ✅ CORRECT
```ninox
"Compare date values with date values"
if Deadline < date(2024, 12, 31) then
    "Correct";
end

"Or compare with today()"
if Deadline < today() then
    "Overdue!";
end
```

**Reason**: Ninox date fields contain date values, not strings. See `references/datetime-functions.md`.

---

## Mistake 18: Using format() Result for Date Comparison

### ❌ WRONG
```ninox
"format() returns TEXT — text comparison is NOT date comparison!"
let d := format(today(), "YYYY-MM-DD");
if d < format(Deadline, "YYYY-MM-DD") then
    "String comparison — wrong!";
end
```

### ✅ CORRECT
```ninox
"Compare date values directly"
if today() < Deadline then
    "Correct date comparison";
end
```

**Reason**: `format()` always returns text. Even if it looks like a date, it's a string.

---

## Mistake 19: Using alert() Outside Button Triggers

### ❌ WRONG
```ninox
"In an onChange or onSave trigger — alert() does NOT work here!"
alert("Record saved!");
```

### ✅ CORRECT
```ninox
"alert() only works in button triggers (user-initiated)"
"In auto-triggers, write to a field instead:"
this.'Status Message' := "Processed at " + format(now(), "DD.MM.YYYY HH:mm");
```

**Reason**: `alert()`, `openRecord()`, `popupRecord()` are UI functions only available in button contexts. See `references/triggers.md`.

---

## Mistake 20: Mixing Types Without Conversion in Concatenation

### ❌ WRONG
```ninox
"Risky: mixing text and number with +"
"Value: " + 42
"Count: " + count(select Orders)
```

### ✅ CORRECT
```ninox
"Always convert explicitly to text"
"Value: " + text(42)
"Count: " + text(count(select Orders))
```

**Reason**: Always use `text()` when concatenating non-text values.

---

## Mistake 21: String Sorting with `order by` Gives Wrong Results

### ❌ WRONG (unexpected results with mixed-case text)
```ninox
"order by sorts strings by character code, not alphabetically"
"Uppercase letters come BEFORE lowercase in ASCII"
let sorted := (select Products) order by Name;
"'ABcD' becomes sorted as 'ABDc' — unexpected!"
```

### ✅ CORRECT: Normalize case before sorting
```ninox
"Add upper() or lower() for consistent alphabetical sort"
let sorted := (select Products) order by lower(Name);

"Or use sort() on extracted values (works for text arrays)"
let names := sort((select Products)[Name]);
```

**Rule**: `order by` sorts by character code index (uppercase before lowercase). Always normalize case for text sorting. Numeric sorting with `order by` works reliably.

---

## Mistake 22: Setting a Relation Field to null Causes Error

### ❌ WRONG
```ninox
"Setting a reference field to null causes 'Expression must return a number or record id: null'"
this.Customer := null;
```

### ✅ CORRECT
```ninox
"Use 0 to clear a reference field"
this.Customer := 0;

"Or use an empty collection"
this.Customer := [];
```

**Reason**: Reference fields expect a record ID (number) or a collection — not `null`. Use `0` or an empty collection to clear a relation field.

---

## Mistake 23: Forgetting to Check `record()` Exists Before Accessing Fields

### ❌ WRONG
```ninox
"record() always returns an rid object — even if ID doesn't exist"
let r := record(Orders, someId);
r.Status := "Done";
"If record doesn't exist, this silently fails or errors"
```

### ✅ CORRECT
```ninox
let r := record(Orders, someId);
if r._id != null then
    r.Status := "Done";
else
    alert("Record not found: " + text(someId));
end
```

**Reason**: `record(table, id)` always returns an rid (record identifier), even for non-existent IDs. Always check `._id != null` before accessing fields.

---

## Mistake 24: Using `http()` Inside a `do as transaction` Block

### ❌ WRONG
```ninox
do as transaction
    "http() inside a transaction — causes errors"
    let response := http("POST", "https://api.example.com/", {}, {});
    this.Result := response.result;
end
```

### ✅ CORRECT
```ninox
"http() must be outside the transaction, or in do as server"
let response := do as server
    http("POST", "https://api.example.com/", {}, {})
end;
do as transaction
    this.Result := response.result;
    this.Status := "Done";
end
```

**Reason**: `http()` is an I/O operation and cannot run inside `do as transaction`. Perform HTTP requests before/outside the transaction, then use the results within.

---

## Mistake 25: Calling `dialog()` or `alert()` from Server-Side Code

### ❌ WRONG
```ninox
"dialog() and alert() in 'do as server' block — does NOT work"
do as server
    alert("Done!");
    let answer := dialog("Confirm", "Are you sure?", ["Yes", "No"]);
end
```

### ✅ CORRECT
```ninox
"UI functions must be outside do as server"
let answer := dialog("Confirm", "Are you sure?", ["Yes", "No"]);
if answer = "Yes" then
    let result := do as server
        "server-side work here"
        http("POST", url, headers, body)
    end;
    alert("Done: " + result.result);
end
```

**Reason**: `do as server` runs code on the server with no UI access. `alert()`, `dialog()`, `openRecord()`, and `openPage()` are client-only UI functions. Call them outside server blocks.

---

## Checklist for Error Prevention

Before every script check:

- [ ] **Field names clear?** If not unambiguous → ASK (see `references/context-queries.md`)
- [ ] **Table names clear?** If not unambiguous → ASK
- [ ] **Relations clear?** If context unclear → ASK
- [ ] Use `select ... where` instead of subsequent filtering
- [ ] Avoid `select` in loops
- [ ] With relations: Iterate directly over the relation, not with `select ... where` in the for loop
- [ ] Use `do as transaction` for multiple write operations
- [ ] Use aggregate functions instead of loops
- [ ] Use only documented functions
- [ ] Label undocumented features
- [ ] Validate inputs
- [ ] Use `limit` for large datasets
- [ ] Avoid nested selects
- [ ] Use `let my := this;` at the beginning to avoid this context problems
- [ ] Use table names instead of relation fields in `select` statements
- [ ] Use `number(record)` for dynamic selection fields
- [ ] Check with flag fields against duplicate execution of operations
- [ ] Never compare dates with strings — use `date()` or `today()`
- [ ] `alert()` only in button triggers — not in onChange/onSave
- [ ] Always use `text()` when concatenating numbers/dates into strings
- [ ] For text sorting: normalize with `lower()` or `upper()` before `order by`
- [ ] To clear a relation/reference field: use `0` or `[]`, NOT `null`
- [ ] After `record(table, id)`: always check `._id != null` before accessing
- [ ] Never use `http()` inside `do as transaction`
- [ ] Never use `dialog()` / `alert()` inside `do as server`

---

**Sources**:
- Official Documentation: https://forum.ninox.de/category/docs
- Performance Rules: `references/performance-rules.md`
- Forbidden Patterns: `references/forbidden-patterns.md`
- Context Queries: `references/context-queries.md` - When to ask instead of guessing
