# Getting Started

This page covers installing the go-rest-client terminal API client, sending your first request, and where it fits among other API clients.

## Prerequisites

| Item | Requirement |
|------|-------------|
| Go | 1.25.1 or higher (declared in `go.mod`; only needed to build from source or `go install`) |
| Terminal | Any terminal supported by `tcell`, which handles rendering |
| Network | Access to the target APIs in your `.http` file |

## Installation

### From Source

```bash
git clone https://github.com/pardnchiu/go-rest-client.git
cd go-rest-client
go build -o gorc ./cmd/tui
sudo cp gorc /usr/local/bin/gorc
```

### Using go install

```bash
go install github.com/pardnchiu/go-rest-client/cmd/tui@latest
sudo cp "$(go env GOPATH)/bin/tui" /usr/local/bin/gorc
```

`go install` names the binary `tui` after the package directory; copying it as `gorc` is only for convenience, and the rest of this site refers to it as `gorc`.

## First Run

Create `api.http`:

```http
### Get User Info
GET https://api.github.com/users/pardnchiu
Accept: application/json

###
```

Launch:

```bash
gorc api.http
```

The left pane lists "Get User Info" and the right pane previews the request line and headers. Press <kbd>Enter</kbd> to send; the right pane then shows the status, response headers, duration, and the formatted JSON body. Press <kbd>Esc</kbd> or <kbd>Ctrl+C</kbd> to exit.

## Where go-rest-client Fits

Posting and ATAC are full terminal API clients that keep requests in their own collection files (YAML for Posting, JSON or YAML for ATAC) with environments and import tools. resterm also runs plain `.http` files in the terminal and adds GraphQL, gRPC, WebSocket, scripted tests, and a CI runner. The VS Code REST Client extension runs `###`-separated `.http` request blocks inside the editor. go-rest-client stays small: one Go binary opens one `.http` file, lists its requests, re-parses the file 200ms after each save, and shows `text/event-stream` responses line by line. Requests are sent exactly as written; `{{variable}}` placeholders are not expanded.

## Next Steps

- Write more requests: [.http File Format](/http-file-format)
- Learn the keys: [Keyboard Controls](/keyboard-controls)
- Edit while testing: [Hot Reload](/hot-reload)
