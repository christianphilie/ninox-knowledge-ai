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

- `concat(collection)` - Concatenate all items in a collection into one string
- `contains(text, search)` - Returns boolean: true if text contains search (case-sensitive)
- `contains(collection, value)` - Returns boolean: true if collection contains value
- `startsWith(text, prefix)` - Returns boolean: true if text starts with prefix
- `endsWith(text, suffix)` - Returns boolean: true if text ends with suffix
- `length(text)` - Text length as number; also works on arrays: `length([a,b,c])` = 3
- `upper(text)` - Convert to uppercase
- `lower(text)` - Convert to lowercase
- `trim(text)` - Remove leading and trailing whitespace ✅ confirmed documented
- `split(text, delimiter)` - Split text at delimiter, returns collection of text
- `splitx(text, regex)` - Split using regular expression
- `substr(text, start)` - Extract from start to end (0-indexed)
- `substr(text, start, length)` - Extract substring (0-indexed start, length characters)
- `substring(text, start, end)` - Extract by start and end position (not length)
- `index(text, search)` - Find position of search in text (0-indexed), -1 if not found
- `index(collection, value)` - Find position of value in collection
- `replace(text, search, replacement)` - Replace ALL occurrences (case-sensitive)
- `replacex(text, regex, replacement)` - Replace using regular expression
- `extractx(text, regex)` - Extract substring using regular expression
- `testx(text, regex)` - Returns boolean if text matches regular expression
- `join(collection, separator)` - Join collection items with separator into text
- `lpad(text, length, padding)` - Left-pad text to length with padding character
- `rpad(text, length, padding)` - Right-pad text to length with padding character
- `capitalize(text)` - Capitalize first letter of each word
- `html(value)` - Returns rich text / HTML representation
- `raw(value)` - Returns raw internal text representation
- `chosen(value, selection)` - Returns boolean if value equals selection

**`lpad()` and `rpad()` examples:**
```ninox
"Left-pad invoice number with zeros to 6 digits"
lpad(text(number(this)), 6, "0")
"→ '000042' for record 42"

"Right-pad a code to fixed length with dashes"
rpad(Code, 8, "-")
"→ 'ABC-----' for Code = 'ABC'"

"Combined: extract and pad"
rpad(substr(LastName, 0, 4), 4, "-") + substr(SSN, 5)
```

**`capitalize()` example:**
```ninox
"Capitalize each word in a name"
capitalize("frank böhmer")
"→ 'Frank Böhmer'"

"Use on input to normalize user-entered names"
this.Name := capitalize(this.Name);
```

**`testx()` example (regex match):**
```ninox
"Test if text matches a regex pattern"
testx("hello@example.com", "^[^@]+@[^@]+\.[^@]+$")
"→ true (looks like an email)"

testx("not-an-email", "^[^@]+@[^@]+\.[^@]+$")
"→ false"

"Case-insensitive match"
testx("Hello World", "hello", "i")
"→ true"
```

**`replacex()` example (regex replace):**
```ninox
"Remove all non-digit characters from phone number"
replacex(PhoneNumber, "\D", "", "g")
"→ '004915112345678' from '+49 (151) 123-45678'"

"Replace all whitespace sequences with single space"
replacex(text, "\s+", " ", "g")
```

**`splitx()` example (regex split):**
```ninox
"Split on one or more whitespace characters"
splitx("word1   word2\tword3", "\s+")
"→ ['word1', 'word2', 'word3']"

"Split on comma or semicolon"
splitx("a,b;c,d", "[,;]")
"→ ['a', 'b', 'c', 'd']"
```

**String concatenation**: The `+` operator concatenates strings. Always use `text()` to convert non-strings: `"Name: " + text(42)`

**Source**: Ninox Documentation - Functions, https://docs.ninox.com/en/script/text-functions
- testx: https://forum.ninox.com/t/60yznqv/testx
- replacex: https://forum.ninox.com/t/35yzw8a/replacex
- lpad: https://forum.ninox.com/t/x2yzbj2/lpad
- capitalize: https://forum.ninox.com/t/p8yzg6k/capitalize

---

## Date and Time Functions

- `today()` - Today's date (type: date)
- `now()` - Current date+time (type: timestamp/datetime)
- `date(year, month, day)` - Create date from components
- `date(value)` - Extract date from datetime/timestamp
- `date(milliseconds)` - Convert Unix milliseconds to date
- `time(hour, minute)` - Create time
- `time(hour, minute, second)` - Create time with seconds
- `time(hour, minute, second, millisecond)` - Full time precision
- `time(value)` - Extract time component from datetime
- `datetime(date, time)` - Combine date and time
- `datetime(year, month, day)` - Create datetime
- `datetime(year, month, day, hour, minute)` - Full datetime
- `datetime(milliseconds)` - Convert Unix milliseconds to datetime
- `year(date)` - Extract year (number)
- `month(date)` - Extract month 1–12 (number)
- `day(date)` - Extract day 1–31 (number)
- `weekday(date)` - Day of week: **0=Monday, 6=Sunday**
- `week(date)` - Calendar week 1–53 (number)
- `quarter(date)` - Quarter 1–4 (number)
- `age(date)` - Full years between date and today (e.g., `age(Birthday)`)
- `workdays(date1, date2)` - Working days (Mon–Fri) between two dates
- `start(appointment)` - Start datetime of appointment field
- `endof(appointment)` - End datetime of appointment field
- `format(date, formatString)` - Format date as text
- `format(date, formatString, language)` - Format with locale (e.g., "de", "en")
- `format(number, formatString)` - Format number as text

**⚠️ weekday() uses 0=Monday, 6=Sunday — NOT 1=Monday, 7=Sunday!**

**Source**: Ninox Documentation - Functions, https://docs.ninox.com/en/script/dates-and-times

---

## Mathematical Functions

- `abs(number)` - Absolute value
- `round(number, decimals)` - Round (decimals is optional, rounds to integer if omitted)
- `floor(number)` - Round down to nearest integer
- `ceil(number)` - Round up to nearest integer
- `sqrt(number)` - Square root
- `pow(base, exponent)` - Power (e.g., `pow(2, 10)` = 1024)
- `max(value1, value2, ...)` - Maximum of multiple values
- `min(value1, value2, ...)` - Minimum of multiple values

**Arithmetic Operators**:
- `+` - Addition (also text concatenation)
- `-` - Subtraction
- `*` - Multiplication
- `/` - Division
- `mod` - Modulo (remainder), e.g., `7 mod 3` = 1

**Source**: Ninox Documentation - Functions, https://docs.ninox.com/en/script/math-functions

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

## Type Conversion Functions

- `text(value)` - Convert any value to text (also: `string(value)` is an alias)
- `number(value)` - Convert value to number (also works on records: returns record ID)
- `boolean(value)` - Convert value to boolean
- `string(value)` - Alias for `text()`, converts to string

**Source**: Ninox Documentation

---

## Collection / Array Functions

- `array(...)` - Create array/collection from values
- `item(collection, index)` - Get element at index position (0-indexed); also `item(json, key)` for JSON
- `unique(collection)` - Remove duplicates from collection ✅ confirmed documented
- `sort(collection)` - Sort collection ascending ✅ confirmed documented
- `rsort(collection)` - Sort collection descending ✅ confirmed documented
- `range(start, end)` - Create numeric array [start, start+1, ..., end-1] (same as `for i from start to end`)
- `slice(collection, start)` - Extract subrange from array or string (0-indexed, inclusive start)
- `slice(collection, start, end)` - Extract subrange with end position (exclusive end)
- `numbers(choiceField)` - Returns IDs of selected values in a multiple choice field
- `files(record)` - Returns all file attachments of a record as array

**`range()` example:**
```ninox
"Create sequence 1 through 5"
for i in range(1, 6) do
    "Process i = 1, 2, 3, 4, 5";
end
```

**`slice()` example:**
```ninox
"Extract elements 1 and 2 from array (0-indexed, exclusive end)"
slice(["Apple","Bagel","Cake","Donut"], 1, 3)
"→ Bagel, Cake"

"Extract characters from text"
slice("Mermaid", 3, 7)
"→ maid"

"Get last 3 records from a sorted selection"
let allOrders := select Orders order by Date desc;
let recent := slice(allOrders, 0, 3);
```

**`numbers()` example:**
```ninox
"Get IDs of selected choices in a multiple choice field"
let selectedIds := numbers('Favorite sports');
"→ e.g. [1, 3, 4] for Basketball, Dancing, Sailing"

"Iterate over selected dynamic choice records"
for id in numbers('Selected Products') do
    let product := record(Products, id);
    "Process product";
end
```

**`files()` example:**
```ninox
"Count files attached to current record"
count(files(this))

"Send all attachments via email"
sendEmail({
    from: "sender@example.com",
    to: "recipient@example.com",
    subject: "Documents",
    text: "Please find the documents attached.",
    attachments: files(this)
})

"Get files from a related record"
let docs := files(this.Customer);
```

**Source**: Ninox Documentation, forum.ninox.com
- numbers(): https://forum.ninox.com/t/q6yz3dl/numbers
- files(): https://forum.ninox.com/t/y4yzrb5
- slice(): https://forum.ninox.com/t/p8yzwmy

---

## UI / Navigation Functions (Button Triggers Only)

These functions are only available in button/click trigger contexts. They do NOT work in onChange, onSave, or formula fields.

- `alert(message)` - Show a message popup to the user (only last call executes if called multiple times)
- `dialog(title, message, [options])` - Show dialog with answer options, returns selected answer (text)
- `openRecord(record)` - Navigate to and open a specific record
- `openTable(tableName)` - Navigate to a table
- `openTable(tableName, viewName)` - Navigate to a specific view of a table
- `openPage(pageName)` - Close current page and open the named page
- `openPage(pageName, tabName)` - Open page at a specific tab
- `openURL(url)` - Open a URL in browser
- `popupRecord(record)` - Open a record in a popup window
- `popupRecord(record, tabName)` - Open record popup at specific tab
- `printRecord(record)` - Print a record
- `printTable(tableName)` - Print a table

**`dialog()` example:**
```ninox
"Button trigger only"
let answer := dialog(
    "Delete record",
    "Are you sure you want to delete this record? This cannot be undone.",
    ["Yes, delete it", "No, keep it"]
);
if answer = "Yes, delete it" then
    delete this
end
```

**`openPage()` example:**
```ninox
"Button trigger only"
openPage("Dashboard")
openPage("Invoices", "Current month")
```

## Platform Detection

- `ninoxApp()` - Returns current platform as text: `"web"`, `"mac"`, `"iphone"`, `"ipad"`, `"android"`, `"tab"`, `"server"`

**`ninoxApp()` example:**
```ninox
"Adjust display based on platform"
if ninoxApp() = "web" then
    "Show web-only content"
end

"Platform-specific greeting"
let platform := ninoxApp();
if platform = "iphone" or platform = "ipad" then
    alert("Use landscape mode for best experience")
end
```

**Source**: Ninox Documentation

---

## User and Session Functions

- `user()` - Returns the currently logged-in user
- `user(name)` - Returns user by name (empty if not found)
- `userId()` - Returns internal alphanumeric ID of current user
- `userId(user)` - Returns ID of given user
- `userName()` - Returns display name of current user (e.g., "Steve Rogers")
- `userName(user)` - Returns name of given user
- `userEmail()` - Returns email of current user
- `userEmail(user)` - Returns email of given user
- `userRole()` - Returns role of current user (e.g., "admin", "editor")
- `userRole(user)` - Returns role of given user
- `userRoles()` - Returns array of all roles of current user
- `userRoles(user)` - Returns all roles of given user
- `userHasRole(role)` - Returns true if current user has given role
- `userHasRole(user, role)` - Returns true if given user has role
- `users()` - Returns all users in the workspace
- `clientLang()` - Returns current language code (e.g., "de", "en", "fr")

**Source**: Ninox Documentation, https://docs.ninox.com/en/script/globals

---

## Global Context Functions

- `teamId()` - Returns ID of current workspace/team
- `databaseId()` - Returns ID of current database
- `tableId(nid)` - Returns table ID (capital letter) from a record or table name
- `tableId(string)` - Returns table ID by table name
- `fieldId(field)` - Returns field ID (e.g., "A", "B")

**`teamId()` / `databaseId()` example:**
```ninox
"Build Ninox REST API URL for the current database"
let baseUrl := "https://api.ninox.com/v1/teams/" + teamId() + "/databases/" + databaseId();
let response := do as server
    http("GET", baseUrl + "/tables", {"Authorization": "Bearer " + ApiKey})
end;
```

**`clientLang()` example (also relevant to User Functions section):**
```ninox
"Show message in user's language"
let msg := if clientLang() = "de" then
    "Datensatz gespeichert"
else if clientLang() = "fr" then
    "Enregistrement sauvegardé"
else
    "Record saved"
end;
alert(msg)
```

**Source**: Ninox Documentation

---

## Database Navigation

- `record(table, number)` - Get a record by table and numeric ID; always returns rid (check `._id` for existence)

**`record()` example:**
```ninox
"Get a specific record by table name and numeric ID"
let customer := record(Customers, 42);
customer.Name
"→ the Name field of customer record 42"

"Check if a record exists before accessing it"
let r := record(Orders, someId);
if r._id != null then
    "Record exists"
    r.Status := "Processed";
end

"Use record ID from a field or variable"
let orderId := number(this.Order);
let order := record(Orders, orderId);
```

**Source**: Ninox Documentation, https://forum.ninox.com/t/y4yzd5c/record

---

## Email

- `sendEmail(options)` - Send an email
  - Required: `from`, `to`, `subject`, `text`
  - Optional: `cc`, `bcc`, `replyTo`, `html`, `attachments`

```ninox
sendEmail({
    from: "sender@example.com",
    to: "recipient@example.com",
    subject: "Subject line",
    text: "Plain text body",
    html: "<b>HTML body</b>",
    attachments: this.Invoice
})
```

**Source**: Ninox Documentation

---

## JSON / HTTP Functions

- `http(method, url)` - HTTP request (returns `{result: ..., error: ...}`)
- `http(method, url, headers)` - With headers
- `http(method, url, headers, body)` - With body
- `http(method, url, headers, body, files)` - With file upload
- `parseJSON(string)` - Parse a JSON string into a Ninox object
- `formatJSON(object)` - Convert a Ninox object to JSON string
- `url(baseUrl, params)` - Build URL with query parameters (returns a clickable link type)
- `urlEncode(string)` - URL-encode a string
- `urlDecode(string)` - URL-decode a string

**`parseJSON()` example:**
```ninox
"Parse a JSON response from http()"
let response := do as server
    http("GET", "https://api.example.com/data", {"Authorization": "Bearer TOKEN"})
end;
if not response.error then
    let data := parseJSON(response.result);
    let name := item(data, "name");
    "Use data.field or item(data, 'key')"
end

"Parse inline JSON (use double-double-quotes for embedded quotes)"
let obj := parseJSON("{""key"": ""value"", ""count"": 42}");
item(obj, "count")
"→ 42"
```

**`formatJSON()` example:**
```ninox
"Convert object to JSON string for API body"
let body := {
    name: this.Name,
    email: this.Email,
    active: true
};
let json := formatJSON(body);
"→ '{"name":"Acme","email":"info@acme.com","active":true}'"

"Log an API response for debugging"
this.DebugLog := formatJSON(response.result);
```

**`url()` example:**
```ninox
"Create a clickable link (use in formula field)"
url("https://ninox.com")

"Build URL with query parameters (automatically URL-encoded)"
url("https://maps.example.com/search", {q: this.Address, zoom: 14})
"→ clickable link: https://maps.example.com/search?q=Main%20Street&zoom=14"
```

**`urlEncode()` / `urlDecode()` example:**
```ninox
"Encode a search term for a URL"
"https://example.com/search?q=" + urlEncode(this.SearchTerm)
"→ 'https://example.com/search?q=Frank%20B%C3%B6hmer'"

"Decode an encoded URL parameter"
urlDecode("Frank%20B%C3%B6hmer")
"→ 'Frank Böhmer'"
```

**Source**: Ninox Documentation
- parseJSON: https://forum.ninox.com/t/p8yzd16
- formatJSON: https://forum.ninox.com/t/p8yzrb4/formatjson
- urlEncode: https://forum.ninox.com/t/q6yz7xc/urlencode
- url(): https://forum.ninox.com/t/m1yz7x7/url

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

**Last Updated**: Expanded with UI functions (alert, openRecord), user(), mod operator, type conversion details, and collection utilities.
