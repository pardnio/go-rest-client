# .http File Format

This page explains how the parser splits request blocks, recognizes request lines, and skips comments; see [Headers and Body](/http-file-format-headers-body) for how headers and bodies are detected.

## Request Blocks

A standalone `###` line ends the current request and adds it to the list; a `### Name` line only sets the name shown in the left pane for the **next** request and does **not** end the current one. An unterminated request at end of file is still included.

```http
### Create Post
POST https://jsonplaceholder.typicode.com/posts
Content-Type: application/json

{
  "title": "Test Post",
  "userId": 1
}

###
```

A request without a `### Name` line shows an empty name in the left pane.

## Request Line

A request line must match `^(GET|POST|PUT|DELETE|PATCH|HEAD|OPTIONS)\s+(.+)$`:

| Rule | Description |
|------|-------------|
| Method | Only the 7 methods above, uppercase only; `get` is not recognized |
| URL | **Everything** after the method; a trailing `HTTP/1.1` becomes part of the URL |
| Position | A request line starts a new request and resets header and body state; the previous request is added to the list only if it already has a body, otherwise it is discarded, so end every request with `###` |

## Comments

Whether a line is skipped depends on whether the parser is inside a body:

| Line | Outside a body | Inside a body |
|------|----------------|---------------|
| Blank line | Skipped | Kept as body content |
| Starts with `//` or `--` | Skipped | Kept as body content |
| Starts with `#` (but not `###`) | Skipped | Skipped, never added to the body |
| `### Name` | Sets the next request's name | Sets the next request's name, never added to the body |
