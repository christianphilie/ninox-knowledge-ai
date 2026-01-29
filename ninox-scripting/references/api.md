# Ninox Automation - API

## Overview

This documentation describes the API functionality in Ninox.

**Source**: https://forum.ninox.de/category/docs

---

## HTTP Function

### Basic HTTP Request
```ninox
http(url, options)
```

### GET Request
```ninox
let response := http("https://api.example.com/data", {
  method: "GET",
  headers: {
    "Authorization": "Bearer token"
  }
});
```

### POST Request
```ninox
let response := http("https://api.example.com/data", {
  method: "POST",
  headers: {
    "Content-Type": "application/json"
  },
  body: {
    "key": "value"
  }
});
```

**Source**: Ninox API Documentation

---

## REST API

### Authentication
The Ninox REST API uses Bearer tokens for authentication.

### Endpoints
The API endpoints vary depending on cloud type (Public Cloud, Private Cloud, On-Premises).

**Source**: https://forum.ninox.de/category/api

---

## Integration with External Tools

### Zapier
Ninox can be integrated with Zapier.

### Make (formerly Integromat)
Integration via REST API possible.

**Source**: Ninox API Documentation

---

## Important Notes

1. **Authentication**: Always use correct authentication
2. **Error Handling**: HTTP requests can fail, handle accordingly
3. **Rate Limits**: Be aware of possible API rate limits

---

**Sources**:
- Official Documentation: https://forum.ninox.de/category/api
- API Reference: https://forum.ninox.de
