# Request Errors

This page explains what the info pane and hint bar show when a request cannot be built, fails to connect, or fails to read.

## Error Types

| Case | Info pane | Hint bar |
|------|-----------|----------|
| Request cannot be built (malformed URL, etc.) | `Error creating request: ...` | Stays at `Sending: name` |
| Connection failure, timeout | `Error: ...` | Red error message |
| Body read failure | Status and headers as usual; body area shows `failed read body: ...` | Status code and duration |

## Timeout

Each request is bounded by both a 120-second `context` timeout and `http.Client.Timeout`. A timeout before response headers arrive shows `Error: ...`; a timeout while reading the body shows `failed read body: ...`. See [Response Display](/response-display) for the regular layout and [SSE Streaming](/sse-streaming) for streams.
