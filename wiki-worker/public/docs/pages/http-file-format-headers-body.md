# Headers and Body

This page explains how each line after the request line is classified as a header or as body.

## Header

After the request line and before the body starts, a line containing `:` is split at the first `:` into key and value (both trimmed). The key must pass `checkHeader`:

| Condition | Example |
|-----------|---------|
| Starts with an ASCII letter | `X-Api-Key` passes, `1Key` fails |
| Contains only letters, digits, `-`, `_` | `Content_Type` passes, `Content Type` fails |
| Line does not start with `"` | `"key": "value"` is always body |

A line without `:`, or one whose key fails, starts the body. A plain-text body whose first line looks like `Key: value` is therefore read as a header. Headers are stored in a `map`, so the last line wins for duplicate keys.

## Body

Blank lines between the headers and the body are skipped and do not start the body. The body starts at the first line matching one of these triggers and continues until the next `###` or request line:

| Trigger | Description |
|---------|-------------|
| `{` or `[` on its own line | Multi-line JSON; this line is the first body line |
| A line that is not a valid header | For example single-line JSON `{"id": 1}` or form data `name=value`; this line is appended to the body twice, so a one-line body is sent as `{"id": 1}\n{"id": 1}`. Put `{` or `[` on its own line to send JSON as written |

Inside the body, lines are kept raw (indentation preserved) and joined with `\n`; blank lines and lines starting with `//` or `--` are content, while lines starting with `#` (but not `###`) are dropped.

See [.http File Format](/http-file-format) for request block and comment rules.
