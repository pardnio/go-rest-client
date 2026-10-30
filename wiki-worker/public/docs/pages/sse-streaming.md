# SSE Streaming

This page explains how Server-Sent Events (SSE) responses are detected and displayed live.

## Detection

Streaming mode starts when the response `Content-Type` contains `text/event-stream`; only the response matters, whether the request sends an Accept: text/event-stream header makes no difference.

```http
### SSE Stream
GET https://sse.dev/test
Accept: text/event-stream

###
```

## Live Rendering

| Behavior | Description |
|----------|-------------|
| Line-by-line append | `bufio.Scanner` redraws the info pane on every line; raw lines are shown as-is (including `data:` and `event:` prefixes) |
| Auto-scroll | Scrolls to the bottom after each redraw |
| Display limit | Only the last 100 lines are kept on screen |
| Header line | After the status and headers, `Duration: elapsed \| Events: count` updates on every line |

`Events` is the number of lines read divided by 2, assuming each event is one data line plus one blank line; multi-line `data:` or events with `event:`/`id:` lines make the count diverge from the real event count.

## Completion

When the server closes the connection, the hint bar shows `(status) METHOD URL | total duration`; on a read error it shows a red `Stream error: ...` and keeps what was received in the info pane.

## Timeout

The HTTP client `Timeout` is fixed at 120 seconds and covers reading the body.
