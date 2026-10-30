# Internal Packages

This page documents the exported types and functions of `internal/ui` and `internal/parser` for anyone modifying or extending the source; both live under `internal/` and cannot be imported from other modules.

## internal/ui

### TUI

```go
type TUI struct {
    App       *tview.Application
    Pages     *tview.Pages
    LeftView  *tview.List
    RightView *tview.TextView
    HintView  *tview.TextView
    Watcher   *fsnotify.Watcher
    Filepath  string
    Requests  []*Request
    Mu        sync.RWMutex
}
```

| Field | Description |
|-------|-------------|
| `App` | The `tview` application; response rendering and hint updates from background goroutines go through `App.QueueUpdateDraw` |
| `Pages` | Created but never added to the layout; currently unused |
| `LeftView` | Request list (`API` pane) |
| `RightView` | Request preview and response (`Info` pane) |
| `HintView` | One-row hint bar at the bottom |
| `Watcher` | `fsnotify` watcher assigned by `main` |
| `Filepath` | Path of the loaded `.http` file, set by `parser.ReadFile` |
| `Requests` | Parse result; hold `Mu` when reading or writing |
| `Mu` | Read-write lock guarding `Requests` |

### Request

```go
type Request struct {
    Name    string
    Method  string
    URL     string
    Headers map[string]string
    Body    string
}
```

The parse result of one `.http` request block; the last entry wins for duplicate `Headers` keys.

### Functions and Methods

| Symbol | Signature | Description |
|--------|-----------|-------------|
| `NewTUI` | `func NewTUI() *TUI` | Builds the two-pane layout and hint bar and registers global keys (<kbd>Tab</kbd>, <kbd>←</kbd>, <kbd>→</kbd>, <kbd>Ctrl+C</kbd>, <kbd>Esc</kbd>) |
| `UpdateLeftView` | `func (t *TUI) UpdateLeftView()` | Rebuilds the list from `Requests`, binds <kbd>Enter</kbd> to send and cursor moves to preview, and previews the first request |

## internal/parser

| Symbol | Signature | Description |
|--------|-----------|-------------|
| `ReadFile` | `func ReadFile(t *ui.TUI, path string) ([]*ui.Request, error)` | Sets `t.Filepath`, parses the whole file, writes `t.Requests` under `Mu`, and calls `UpdateLeftView` on the calling goroutine; returns a wrapped error when opening or reading fails, leaving `t.Requests` unchanged |
| `WatchFile` | `func WatchFile(t *ui.TUI)` | Reads events and errors from `t.Watcher` until the channels close; `fsnotify.Write`/`fsnotify.Create` call `ReloadFile` after a 200ms debounce |
| `ReloadFile` | `func ReloadFile(t *ui.TUI)` | Shows `Edited at`, re-parses `t.Filepath` with `ReadFile`, then in the event loop sets the cursor to the list's current index (0 after the rebuild) and shows `Reloaded at`; on failure shows the error in red |

See [.http File Format](/http-file-format) for syntax and [Hot Reload](/hot-reload) for reload behavior.
