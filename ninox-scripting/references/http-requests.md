# HTTP Requests in Ninox

## Overview

Ninox allows making HTTP requests from scripts using the `http()` function. This is useful for integrating with external APIs, sending webhooks, and fetching external data.

**Critical**: HTTP requests should be wrapped in `do as server` blocks. **Do NOT** use `http()` inside `do as transaction` blocks — this causes errors.

**Source**: Ninox Documentation, https://docs.ninox.com/en/script/http-requests (forum.ninox.com/t/x2yzljz)

---

## The http() Function

### Correct Signature

```ninox
http(method, url)
http(method, url, headers)
http(method, url, headers, body)
http(method, url, headers, body, files)
```

**⚠️ IMPORTANT**: The signature is `http(method, url, ...)` — method is the **first** parameter, NOT inside an options object.

| Parameter | Type | Required | Description |
|---|---|---|---|
| `method` | text | yes | `"GET"`, `"POST"`, `"PUT"`, `"DELETE"` |
| `url` | text | yes | Full URL including protocol |
| `headers` | JSON object | no | HTTP headers as key-value object |
| `body` | JSON / text | no | Request payload (use `{}` if no body needed) |
| `files` | file array | no | Files to upload (server context only) |

### Return Value

`http()` returns a JSON object with either:
- `response.result` — the response body (parsed as JSON if applicable)
- `response.error` — error message if request failed

```ninox
"Always check for error before using result"
let response := do as server
    http("GET", "https://api.example.com/data", {"Authorization": "Bearer token"})
end;
if response.error then
    alert(text(response.error))
else
    "Use response.result"
    let data := response.result;
end
```

---

## Examples

### GET Request
```ninox
do as server
    let response := http("GET", "https://api.example.com/data", {
        "Authorization": "Bearer myApiToken",
        "Accept": "application/json"
    });
    if response.error then
        alert("Error: " + text(response.error))
    else
        let data := response.result;
        "data is the parsed JSON body"
    end
end
```

### GET Request Without Auth
```ninox
let response := do as server
    http("GET", "https://api.example.com/public-data")
end;
if response.error then
    alert(text(response.error))
else
    alert(text(response.result))
end
```

### POST Request with JSON Body
```ninox
let my := this;
let response := do as server
    http("POST", "https://api.example.com/records",
        {
            "Content-Type": "application/json",
            "Authorization": "Bearer myApiToken"
        },
        {
            name: my.Name,
            email: my.Email,
            status: my.Status
        }
    )
end;
if response.error then
    my.'API Error' := text(response.error);
else
    my.'External ID' := text(response.result.id);
end
```

### PUT Request (Update)
```ninox
let my := this;
do as server
    http("PUT",
        "https://api.example.com/records/" + text(my.ExternalID),
        {
            "Content-Type": "application/json",
            "Authorization": "Bearer myApiToken"
        },
        {
            status: my.Status,
            updatedAt: format(now(), "YYYY-MM-DDTHH:mm:ss")
        }
    );
end
```

### DELETE Request
```ninox
let my := this;
do as server
    http("DELETE",
        "https://api.example.com/records/" + text(my.ExternalID),
        {"Authorization": "Bearer myApiToken"}
    );
end
```

### File Upload (multipart)
```ninox
"⚠️ File upload — server context only"
let my := this;
do as server
    http("POST", "https://example.com/upload",
        {"content-type": "multipart/form-data"},
        null,
        [my.Attachment]
    );
end
```

---

## Working with JSON Responses

### Accessing `response.result`
```ninox
let response := do as server
    http("GET", "https://api.example.com/user/123",
        {"Authorization": "Bearer token"})
end;
if not response.error then
    let userData := response.result;
    let userName := userData.name;
    let userEmail := userData.email;
    let userId := userData.id;
end
```

### Nested JSON Fields
```ninox
let response := do as server
    http("GET", "https://api.example.com/data",
        {"Authorization": "Bearer token"})
end;
let city := response.result.address.city;
let zip := response.result.address.zip;
```

### JSON Arrays in Response
```ninox
"If response.result is an array, use item() to access elements"
let response := do as server
    http("GET", "https://api.example.com/items",
        {"Authorization": "Bearer token"})
end;
let items := response.result;
let firstItem := item(items, 0);
let total := count(items);
```

### Parsing JSON Strings
If the response is a JSON string (not auto-parsed), use `parseJSON()`:
```ninox
let response := do as server
    http("GET", "https://api.example.com/data",
        {"Authorization": "Bearer token"})
end;
let parsed := parseJSON(text(response.result));
let value := parsed.fieldName;
```

---

## Authentication Patterns

### Bearer Token
```ninox
{"Authorization": "Bearer YOUR_API_TOKEN_HERE"}
```

### API Key in Header
```ninox
{"X-API-Key": "YOUR_API_KEY", "Accept": "application/json"}
```

### API Key in URL
```ninox
let apiKey := first(select ApiSettings).ApiKey;
let url := "https://api.example.com/data?api_key=" + apiKey;
let response := do as server
    http("GET", url, {})
end;
```

### OAuth2 (Full Example from Ninox Docs)
```ninox
"Full OAuth2 client_credentials flow"
let tokenEndpoint := "https://authserver.example.com/connect/token";
let clientId := first(select ApiSettings).ClientId;
let clientSecret := first(select ApiSettings).ClientSecret;
let body := "client_id=" + clientId + "&client_secret=" + clientSecret +
    "&scope=api_access&grant_type=client_credentials";
let headers := {
    'Content-Type': "application/x-www-form-urlencoded",
    'Content-Length': length(body)
};
let response := do as server
    http("POST", tokenEndpoint, headers, body)
end;
if response.error then
    'Error' := text(response.error)
else
    let json := parseJSON(text(response.result));
    let accessToken := text(json.token_type) + " " + text(json.access_token);
    "Now use accessToken for API calls"
end
```

---

## URL Utilities

### urlEncode() / urlDecode()
```ninox
urlEncode("Test Parameter")    "→ 'Test%20Parameter'"
urlDecode("Test%20Parameter")  "→ 'Test Parameter'"
```

### url() — Build URL with Parameters
```ninox
url("https://api.example.com", {
    page: 1,
    perPage: 20,
    search: "John Doe"
})
"→ 'https://api.example.com?page=1&perPage=20&search=John%20Doe'"
```

---

## Calling the Ninox API FROM a Script

You can call the Ninox REST API from within a Ninox script using `teamId()`, `databaseId()`, `tableId()`:

```ninox
let response := do as server
    http("GET",
        "https://api.ninox.com/v1/teams/" + teamId() +
        "/databases/" + databaseId() +
        "/tables/" + tableId(this) + "/records",
        { Authorization: "Bearer YOUR_NINOX_API_KEY" },
        {}
    )
end;
response.result
```

**Note**: `{}`  (empty body) is required when accessing Ninox data even for GET requests.

---

## Webhook Pattern (Sending Data Out)

```ninox
"onSave or Button trigger — send webhook"
let my := this;
do as server
    http("POST", "https://hooks.zapier.com/hooks/catch/12345/abcdef/",
        {"Content-Type": "application/json"},
        {
            record_id: text(number(my)),
            customer_name: my.Name,
            amount: my.Amount,
            status: my.Status,
            date: format(my.Date, "YYYY-MM-DD")
        }
    );
end
```

---

## Important Constraints

| Rule | Detail |
|---|---|
| ❌ No `http()` in `do as transaction` | Will cause an error — keep them separate |
| ✅ Use `do as server` | Bypasses browser CORS, recommended for all HTTP calls |
| ⚠️ Triggers are already server-side | `do as server` is redundant in triggers but still works |
| ⏱️ Timeout | Server context timeout is ~20–120 seconds |
| 📦 Body required for Ninox API | Include `{}` even for GET requests to Ninox's own API |

---

## Error Handling

```ninox
"Always check response.error"
let response := do as server
    http("POST", "https://api.example.com/data",
        {"Content-Type": "application/json", "Authorization": "Bearer token"},
        {key: "value"}
    )
end;
if response.error then
    "Handle error"
    this.'API Status' := "Error: " + text(response.error);
else
    "Handle success"
    this.'API Status' := "Success";
    this.'API Result' := text(response.result);
end
```

---

## Best Practices

1. **Always use `do as server`** for HTTP requests — bypass CORS and ensure reliability.
2. **Always check `response.error`** before accessing `response.result`.
3. **Store API keys in a settings table** — never hardcode in scripts.
4. **Avoid HTTP calls in loops** — they are slow and can hit rate limits.
5. **Log API calls** to an audit table for debugging.

```ninox
"Best practice: API key from settings table, error logging"
let my := this;
let apiKey := first(select 'API Settings' where Active = true).ApiKey;
let response := do as server
    http("POST", "https://api.example.com/endpoint",
        {"Content-Type": "application/json", "Authorization": "Bearer " + apiKey},
        {id: text(number(my)), name: my.Name}
    )
end;
if response.error then
    my.'API Error' := text(response.error);
    my.'Last API Call' := now();
else
    my.'API Status' := "OK";
    my.'Last API Call' := now();
end
```

---

## Sources

- Official Documentation: https://docs.ninox.com/en/script/http-requests
- Ninox API: https://docs.ninox.com/en/api
- Community: forum.ninox.com/t/x2yzljz, forum.ninox.com/t/q6yzrdp
