# Undocumented but Working Features

These features are **not** described in the official documentation but proven to work. They may be used but must **always** be labeled appropriately.

## Usage Rule

**MANDATORY LABELING**: Always mark with comment:
```
⚠️ Not in official documentation, but works
```

## List of Undocumented Features

### 1. Square Brackets `[]` as Filter After `select`

**Description**: After a `select`, additional filtering can be done with square brackets `[]`.

**Example**:
```ninox
"⚠️ Not in official documentation, but works";
let records := select Table[ Field = "Value" ];
```

**Note**: 
- Works, but performance difference from `select ... where`
- `select Table where Field = "Value"` filters during selection (server-side)
- `select Table[ Field = "Value" ]` fetches all records first, then filtering (client-side)
- Prefer `where` for better performance

**Source**: Community examples in forum

---

### 1a. Square Brackets `[]` as Filter Directly on Relations

**Description**: Relations can be filtered directly with square brackets `[]` without using `select`.

**Syntax**:
```ninox
"⚠️ Not in official documentation, but works";
Relation[ Field = Value ]
```

**Example**:
```ninox
"⚠️ Not in official documentation, but works";
let my := this;
let filteredWarehouseBookings := my.Product.WarehouseBookings[Warehouse = my.'From Source Warehouse'];
let firstBooking := first(filteredWarehouseBookings);
```

**Explanation**:
- `my.Product.WarehouseBookings` is a relation (1:N relationship)
- `[Warehouse = my.'From Source Warehouse']` filters the relation directly
- The result can be further processed with `first()`, `last()`, `count()` etc.
- Direct field access possible: `first(Relation[Filter]).Field`

**Notes**:
- Works directly on relations, not just after `select`
- Very compact and efficient
- Can be combined with `first()`, `last()`, `count()`
- Performance can be affected with very large relations

**Difference from `select Table[Filter]`**:
- `select Table[Filter]` works on tables
- `Relation[Filter]` works directly on relations (without `select`)
- Both use the same `[]` syntax

**Source**: Tested and working, but not found in official documentation

---

### 2. Nested `count()` with `[]` Filter in `where` Clauses

**Description**: In `where` clauses, `count()` can be used with square brackets `[]` to filter and count relation fields.

**Syntax**:
```ninox
"⚠️ Not in official documentation, but works";
select 'Table' where count(RelationField[Condition]) >= Count
```

**Example**:
```ninox
"⚠️ Not in official documentation, but works";
"Finds all orders with at least 5 unbilled service reports";
select 'Orders' where count(ServiceReport[Billed = false]) >= 5
```

**Explanation**:
- `ServiceReport` is a relation field (1:N relationship)
- `[Billed = false]` filters the relation records
- `count()` counts the filtered records
- `>= 5` checks if at least 5 records exist

**Notes**:
- Works with relation fields (1:N relationships)
- `[]` filter is applied within `count()`
- Can also be used with other comparison operators (`=`, `>`, `<`, `<=`, `>=`)
- Performance can be affected with large relations

**Source**: Tested and working, but not found in official documentation

---

### 3. `cnt()` as Alternative to `count()`

**Description**: `cnt()` is an alias for `count()` and works identically.

**Example**:
```ninox
"⚠️ Not in official documentation, but works";
let count := cnt(select Orders);
"Identical to: count(select Orders)";
```

**Notes**:
- Works identically to `count()`
- Frequently used in community examples
- Both variants are interchangeable

**Source**: Community examples in forum

---

### 4. `for i from start to end` - Range Loop

**Description**: Loop over a numeric range without explicit collection.

**Syntax**:
```ninox
"⚠️ Not in official documentation, but works";
for i from start to end do
  "Code";
end
```

**Example**:
```ninox
"⚠️ Not in official documentation, but works";
"Iterates from 7 to 18 (inclusive)";
for hour from 7 to 18 do
  alert("Hour: " + hour);
end
```

**Notes**:
- `start` and `end` must be numbers
- Both boundaries are inclusive
- Can also be used with `for i from 0 to 5`

**Source**: Tested and working, but not found in official documentation

---

### 5. `start()` and `endof()` for Datetime Ranges

**Description**: Functions to extract start and end time from a datetime range.

**Syntax**:
```ninox
"⚠️ Not in official documentation, but works";
start(datetime);
"Start time";
endof(datetime);
"End time";
```

**Example**:
```ninox
"⚠️ Not in official documentation, but works";
let appointment := item.Appointment;
"Datetime range";
let startTime := start(appointment);
let endTime := endof(appointment);
let duration := endTime - startTime;
```

**Notes**:
- Works with datetime fields that represent time ranges
- `start()` returns the beginning
- `endof()` returns the end
- Useful for appointment scheduling and time calculations

**Source**: Tested and working, but not found in official documentation

---

### 6. `extractx()` - Regex-based Extraction

**Description**: Extracts text with regular expressions and replacement patterns.

**Syntax**:
```ninox
"⚠️ Not in official documentation, but works";
extractx(text, pattern, replacement)
```

**Example**:
```ninox
"⚠️ Not in official documentation, but works";
let layoutInfo := ";123:45,60;";
let layoutStr := extractx(layoutInfo, ";" + text(123) + ":(.+?);", "$1");
"Result: '45,60'";
```

**Notes**:
- Uses regular expressions
- `replacement` can contain replacement patterns like `$1`, `$2`
- Useful for complex text processing

**Source**: Tested and working, but not found in official documentation

---

### 7. `popupRecord()` - Open Record in Popup

**Description**: Opens a record in a popup window.

**Syntax**:
```ninox
"⚠️ Not in official documentation, but works";
popupRecord(record)
```

**Example**:
```ninox
"⚠️ Not in official documentation, but works";
let newRecord := create 'Customers / Contacts';
popupRecord(newRecord);
```

**Notes**:
- Opens the record in a popup
- Useful for UI interactions
- Similar to `openRecord()`, but as popup

**Source**: Tested and working, but not found in official documentation

---

### 8. `html()` - HTML Output

**Description**: Outputs HTML code directly that is rendered in the browser.

**Syntax**:
```ninox
"⚠️ Not in official documentation, but works";
html(htmlCode)
```

**Example**:
```ninox
"⚠️ Not in official documentation, but works";
html("<div style='color: red;'>Hello World</div>");
```

**Notes**:
- Outputs HTML directly
- Useful for complex UI elements
- Can contain CSS and JavaScript

**Source**: Tested and working, but not found in official documentation

---

### 9. `color()` - Get Field Color

**Description**: Returns the color of a field (e.g., for selection fields with color assignment).

**Syntax**:
```ninox
"⚠️ Not in official documentation, but works";
color(field)
```

**Example**:
```ninox
"⚠️ Not in official documentation, but works";
let color := color(item.AppointmentType);
"Returns e.g. 'blue' or '#ff0000'";
```

**Notes**:
- Works with fields that have colors assigned
- Returns color value as text (e.g., "blue", "#ff0000")
- Useful for dynamic formatting

**Source**: Tested and working, but not found in official documentation

---

### 10. `formatXML()` - Format XML

**Description**: Formats data as XML.

**Syntax**:
```ninox
"⚠️ Not in official documentation, but works";
formatXML(data, pretty)
```

**Example**:
```ninox
"⚠️ Not in official documentation, but works";
let xml := formatXML(data, true);
"Formats data as XML with indentation (pretty = true)";
```

**Notes**:
- `pretty` determines whether XML is formatted (true) or compact (false)
- Useful for data export and API integrations

**Source**: Tested and working, but not found in official documentation

---

### 11. `number(record)` - Convert Record for Dynamic Selection Fields

**Description**: Converts a record to a numeric value to use it in dynamic selection fields.

**Syntax**:
```ninox
"⚠️ Not in official documentation, but works";
number(record)
```

**Example**:
```ninox
"⚠️ Not in official documentation, but works";
let warehouseLocation := first(select WarehouseLocations where WarehouseManagement = warehouseManagement and User = employeeUser);
let newWithdrawal := (create WarehouseBookings);
newWithdrawal.(
    Warehouse := number(warehouseLocation);  "Converts record to numeric value for dynamic selection field"
)
```

**Problem without `number()`**:
```ninox
"❌ WRONG - Error: Field must return dynamic values";
Warehouse := warehouseLocation;  "Does not work with dynamic selection fields"
```

**Explanation**:
- Dynamic selection fields expect a collection or a numeric value
- Directly setting a record object causes error "Field must return dynamic values"
- `number(record)` converts the record to a numeric value (ID/No)
- This numeric value can then be used in the dynamic selection field

**Notes**:
- Only works if the dynamic selection field is based on the same table as the record
- With relation fields, the record can be used directly: `RelationField := record;`
- Alternative: Collection with one element: `Warehouse := select WarehouseLocations where No = warehouseLocation.No;`

**Source**: Tested and working, but not explicitly found in official documentation

---

### Further Features

This list is continuously expanded when new undocumented but working features are discovered.

## Adding New Features

If you discover a new undocumented but working feature:

1. Test it thoroughly
2. Ensure it works consistently
3. Document it here with:
   - Description
   - Example code
   - Usage notes
   - Source (if available)

## Important

- These features may change or be removed
- No guarantee for future compatibility
- Always prefer documented alternative if available
