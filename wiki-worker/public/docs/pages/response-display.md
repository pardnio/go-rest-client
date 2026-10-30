# Response Display

This page explains how a regular (non-streaming) HTTP response is shown in the info pane and hint bar.

## Layout

```
200 OK
Headers:
  Content-Type: application/json; charset=utf-8
  Duration: 245.123ms

Body:
  {
    "login": "pardnchiu"
  }
```

| Section | Content |
|---------|---------|
| Status line | `resp.Status` (e.g. `200 OK`), colored by status code |
| `Headers:` | One line per response header; multi-value headers span multiple lines; order is not stable |
| `Duration:` | Time from sending to finishing the body read |
| `Body:` | Response body, pretty-printed when it parses as JSON |

## Status Colors

| Status | Color | Applied to |
|--------|-------|------------|
| < 300 | Green | Status line, hint bar status code |
| 300–399 | Yellow | Same |
| ≥ 400 | Red | Same, and the whole body is red |

## JSON Formatting

When the body parses with `json.Unmarshal`, it is re-indented with two spaces; otherwise (HTML, plain text, empty body) it is shown as-is. Formatting affects display only, never the data sent or received.

## Hint Bar

On completion the hint bar shows `(status) METHOD URL | duration`, for example:

```
(200) GET https://api.github.com/users/pardnchiu | 245.123ms
```

See [Request Errors](/response-errors) for what is shown when a request fails.
