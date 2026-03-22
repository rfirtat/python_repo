# Python `requests` Response Object Reference

## Overview

When using the `requests` library to interact with APIs, HTTP requests return a **`Response` object**.

Example:

```python
import requests

response = requests.get("https://api.example.com/data")
```

The variable `response` now contains the **server's full HTTP reply**, including:

* status code
* response headers
* response body (data returned by the API)

Most API work involves **inspecting and parsing the `Response` object**.

---

# Typical Workflow

A common pattern when calling APIs:

```python
import requests

response = requests.get(url, params=params)

# Check if request succeeded
response.raise_for_status()

# Convert JSON response to Python dictionary
data = response.json()

# Extract fields
value = data["some_field"]
```

Typical steps:

1. Send request
2. Check response status
3. Parse response body
4. Extract data

---

# Important Response Attributes and Methods

## `response.status_code`

Returns the **HTTP status code** of the response.

Example:

```python
print(response.status_code)
```

Common codes:

| Code | Meaning            |
| ---- | ------------------ |
| 200  | Request successful |
| 400  | Bad request        |
| 401  | Unauthorized       |
| 404  | Resource not found |
| 500  | Server error       |

Useful for basic error checking.

---

## `response.ok`

Boolean indicating if the request was successful.

Equivalent to:

```python
response.status_code < 400
```

Example:

```python
if response.ok:
    print("Request succeeded")
```

---

## `response.raise_for_status()`

Raises a Python exception if the request failed.

Example:

```python
response.raise_for_status()
```

If the server returned an error (e.g. 404 or 500), this will raise:

```
requests.exceptions.HTTPError
```

This is commonly used for **simple error handling**.

---

## `response.text`

Returns the **raw response body as a string**.

Example:

```python
print(response.text)
```

Useful when working with:

* HTML responses
* plain text APIs
* debugging raw server responses

Example output:

```
'{"temperature":18.7,"windspeed":3.2}'
```

---

## `response.content`

Returns the **raw response body as bytes**.

Example:

```python
print(response.content)
```

Used for:

* downloading files
* images
* binary data

Example:

```python
with open("image.png", "wb") as f:
    f.write(response.content)
```

---

## `response.json()`

Parses a JSON response and converts it into a **Python dictionary**.

Example:

```python
data = response.json()
```

If the API returned:

```json
{
  "temperature": 18.7,
  "windspeed": 3.2
}
```

Python receives:

```python
{
  "temperature": 18.7,
  "windspeed": 3.2
}
```

This allows easy access to values:

```python
temperature = data["temperature"]
```

---

## `response.headers`

Returns the HTTP headers as a dictionary-like object.

Example:

```python
print(response.headers)
```

Example output:

```
{
  "Content-Type": "application/json",
  "Content-Length": "234"
}
```

Useful for checking:

* response format
* rate limit information
* caching headers

---

## `response.url`

Returns the final URL of the request.

Example:

```python
print(response.url)
```

Useful when requests automatically follow redirects.

---

# Debugging Tips

### Pretty-print JSON responses

```python
import json

print(json.dumps(response.json(), indent=4))
```

This formats the JSON for easier reading.

---

### Inspect available fields

```python
data = response.json()

print(data.keys())
```

This shows top-level keys in the response.

---

# Summary

The most commonly used parts of the `Response` object:

| Attribute / Method            | Purpose                           |
| ----------------------------- | --------------------------------- |
| `response.status_code`        | Check HTTP result                 |
| `response.ok`                 | Boolean success indicator         |
| `response.raise_for_status()` | Raise error if request failed     |
| `response.text`               | Raw response as string            |
| `response.content`            | Raw response as bytes             |
| `response.json()`             | Convert JSON to Python dictionary |
| `response.headers`            | Inspect HTTP headers              |
| `response.url`                | Final request URL                 |

These are the primary tools used when **interacting with APIs using `requests`**.
