# Ninox Global Functions and Variables

## Overview

These are globally available functions and identifiers in Ninox NX scripts. They are available in any context (triggers, formulas, buttons) unless otherwise noted.

**Source**: Ninox Documentation, https://docs.ninox.com/en/script/globals

---

## Context Variables

### `this`
Reference to the current record in a trigger or formula context.

```ninox
let my := this;          "Store in variable to avoid context issues"
my.Name                  "Access field"
my.Status := "Active";   "Set field"
```

**Best practice**: Always assign to `let my := this;` at the start of a script.

---

## Database Identification

### `teamId()` → text
Returns the ID of the current workspace/team.
```ninox
teamId()   "→ e.g. 'Ruh6mFq9nbdvhx4mf'"
```

### `databaseId()` → text
Returns the ID of the current database.
```ninox
databaseId()   "→ e.g. 'mcxhedkkmvpp'"
```

### `tableId(nid)` → text
Returns the table letter-ID of a record or table name.
```ninox
tableId(this)          "→ table ID of current record, e.g. 'A'"
tableId("Customers")   "→ table ID by name"
```

### `fieldId(field)` → text
Returns the field ID (letter code).
```ninox
fieldId(this.Name)   "→ e.g. 'A' (first field)"
```

### `raw(record)` → text
Returns the full internal record ID (table letter + record number).
```ninox
raw(this)   "→ e.g. 'K13' (table K, record 13)"
```

---

## User Functions

### `user()` → user
Returns the currently logged-in user. Can compare to User fields.

```ninox
user()                           "→ current user"
user("Steve Rogers")             "→ specific user by name (empty if not found)"
user() = 'Assigned To'           "→ check if current user is assigned"
user() = user("Boris Johnson")   "→ check if specific user"
```

### `userId()` → text
Returns the internal alphanumeric ID of the current user.
```ninox
userId()         "→ e.g. 'abcd1234efgh'"
userId(someUser) "→ ID of a specific user"
```

### `userName()` → text
Returns the display name of the current user.
```ninox
userName()        "→ 'Steve Rogers'"
userName(User)    "→ name of user in User field"
```

### `userEmail()` → text
Returns the email address of the current user.
```ninox
userEmail()        "→ 'steve@example.com'"
userEmail(User)
```

### `userRole()` → text
Returns the primary role of the current user.
```ninox
userRole()         "→ 'admin' or 'editor' or custom role"
userRole(User)
```

Default roles: `"admin"`, `"editor"`. Custom roles can be created in workspace settings.

### `userRoles()` → [text]
Returns all roles of the current user as an array.
```ninox
userRoles()   "→ ['admin', 'Leadership Team']"
```

### `userHasRole(role)` → boolean
Returns true if the current user has the given role.
```ninox
if userHasRole("admin") then
    "Allow access";
end

if userHasRole(user(), "editor") then
    "User is an editor";
end
```

### `users()` → [user]
Returns all users in the workspace.
```ninox
let allUsers := users();
for u in allUsers do
    "Process each user";
end
```

### `clientLang()` → text
Returns the current language code of the browser/app.
```ninox
clientLang()   "→ 'de', 'en', 'fr', 'es', etc."

"Localized greeting example"
if clientLang() = "de" then
    alert("Willkommen!")
else
    alert("Welcome!")
end
```

---

## Performance Execution Blocks

### `do as transaction ... end`
Wraps multiple write operations in a single transaction for better performance.

```ninox
do as transaction
    record1.Field := value1;
    record2.Field := value2;
    record3.Field := value3;
end
```

**Rules**:
- ❌ Do NOT use `http()` inside a transaction block
- ✅ Best for loops with many write operations
- ✅ Ensures all-or-nothing execution

### `do as server ... end`
Executes script server-side. Returns a result.

```ninox
let response := do as server
    http("GET", "https://api.example.com/data", {"Authorization": "Bearer TOKEN"})
end;
response.result
```

**Rules**:
- ✅ Required for HTTP calls from button triggers (bypasses CORS)
- ⚠️ Redundant in table/field triggers (they are already server-side)
- ❌ Cannot execute `alert()`, `openRecord()`, other UI functions
- ⏱️ Timeout: ~20–120 seconds

### `do as deferred ... end`
Splits read from write transaction; allows background processing.

```ninox
"Process immediately, then send email in background"
this.Status := "Sent";
this.'Sent On' := today();
do as deferred
    sendEmail({
        from: "billing@example.com",
        to: this.Customer.Email,
        subject: "Invoice " + this.'Invoice Number',
        text: "Please find your invoice attached."
    })
end
```

---

## Email Function

### `sendEmail(options)` → any
Send an email from a Ninox script.

```ninox
sendEmail({
    from: "sender@example.com",        "Required"
    to: "recipient@example.com",       "Required (or use User field)"
    cc: "copy@example.com",            "Optional"
    bcc: "bcc@example.com",            "Optional"
    replyTo: "reply@example.com",      "Optional"
    subject: "Subject line",           "Required"
    text: "Plain text body",           "Required"
    html: "<b>HTML body</b>",          "Optional — overrides text"
    attachments: this.Invoice          "Optional — file field"
})
```

**Notes**:
- Can be used in any trigger context
- Use `do as deferred` to avoid blocking the user on save
- The `from` address may need to be configured in Ninox settings
- `attachments` accepts a file field value

---

## Print Functions (Button/Client Only)

### `printRecord(record)` → void
Prints a specific record.

### `printTable(tableName)` → void
Prints a table view.

---

## Platform Detection

### `ninoxApp()` → text
Returns a string identifying which platform Ninox is currently running on. Useful for platform-specific UI or logic.

**Return values:**
| Value | Platform |
|---|---|
| `"web"` | Ninox Cloud (browser) |
| `"mac"` | macOS desktop app |
| `"iphone"` | iPhone app |
| `"ipad"` | iPad app |
| `"android"` | Android phone app |
| `"tab"` | Android tablet app |
| `"server"` | Server-side execution |

```ninox
"Show browser-specific field"
if ninoxApp() = "web" then
    "Only visible in browser"
end

"Adjust output depending on platform"
switch ninoxApp() do
    case "web":     "Browser version";
    case "mac":     "Mac app";
    case "iphone":  "iPhone";
    case "ipad":    "iPad";
    default:        "Other platform";
end
```

**Common use**: Add `ninoxApp() = "web"` to the "Display field only, if" condition on a field to show it only in the web app.

**Source**: https://forum.ninox.com/t/g9yzdkd/ninoxapp (official docs redirect)

---

## Navigation Functions (Button/Client Only)

### `openPage(pageName)` → void
### `openPage(pageName, tabName)` → void
Closes the current page and opens the specified page. Optionally opens to a specific tab.

**Parameters:**
- `pageName` — the page name as a text string
- `tabName` (optional) — the tab to open within the page; defaults to the last active tab

```ninox
"Open a page by name"
openPage("Invoices")

"Open a page at a specific tab"
openPage("Invoices", "Current month")
```

**Restrictions:**
- UI function — only works in button (on-click) triggers
- Does NOT work in onChange, onSave, or formula fields
- Known limitation: does NOT work in the iPhone app [COMMUNITY REPORT]

**Source**: https://forum.ninox.com/t/h7yzxtg/openpage (official docs redirect)

---

## Sources

- Official Documentation: https://docs.ninox.com/en/script/globals
- User Functions: https://docs.ninox.com/en/script/globals (user management section)
- ninoxApp(): https://forum.ninox.com/t/g9yzdkd/ninoxapp
- openPage(): https://forum.ninox.com/t/h7yzxtg/openpage
- Community Forum: https://forum.ninox.de
