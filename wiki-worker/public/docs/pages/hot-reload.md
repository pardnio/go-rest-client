# Hot Reload

This page explains how go-rest-client watches the `.http` file and reloads the request list after you save.

## Triggers

On startup, `fsnotify` watches the file given on the command line. A `fsnotify.Write` or `fsnotify.Create` event starts a 200ms timer; any further event during that window resets it, so a burst of editor writes triggers only one reload. `fsnotify.Remove`, `fsnotify.Rename`, and `fsnotify.Chmod` events do not trigger a reload.

## Reload Flow

| Stage | Hint bar | Description |
|-------|----------|-------------|
| Start | Yellow `Edited at HH:MM:SS` | Timer fired |
| Parse | — | `ReadFile` re-parses the whole file |
| Success | Green `Reloaded at HH:MM:SS` | List rebuilt from the new parse; the cursor is on the first request and the info pane previews it |
| Failure | Red `<error> at HH:MM:SS` | List and info pane keep their pre-reload content |

`ReloadFile` reads the list's current index only after `ReadFile` has rebuilt the list, and rebuilding a `tview` list resets its cursor to 0, so after every successful reload the selection is the first request.

## Watch Errors

Errors reported by `fsnotify` appear in the hint bar as red `Watch error: ...`, and the program keeps running.
