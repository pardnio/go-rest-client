# Keyboard Controls

This page lists every key in the TUI and how focus moves between panes.

## Key Map

| Key | Scope | Function |
|-----|-------|----------|
| <kbd>↑</kbd> <kbd>↓</kbd> | Request list | Move the cursor; the right pane previews that request |
| <kbd>Enter</kbd> | Request list | Send the selected request and focus the info pane |
| <kbd>↑</kbd> <kbd>↓</kbd> | Info pane | Scroll the response |
| <kbd>Tab</kbd> | Global | Toggle focus between request list and info pane |
| <kbd>←</kbd> | Global | Focus the request list |
| <kbd>→</kbd> | Global | Focus the info pane |
| <kbd>Ctrl+C</kbd> <kbd>Esc</kbd> | Global | Exit |

<kbd>Tab</kbd>, <kbd>←</kbd>, <kbd>→</kbd>, <kbd>Ctrl+C</kbd>, and <kbd>Esc</kbd> are captured globally by `App.SetInputCapture` and never reach individual widgets; all other keys go to the focused `tview` widget.

## Layout

| Area | Size | Content |
|------|------|---------|
| `API` (left) | 1/3 width | Request names; the selected row is highlighted full-width in `tcell.ColorDarkCyan` |
| `Info` (right) | 2/3 width | Request preview or response; scrollable with word wrap |
| Hint bar (bottom) | 1 row | Current request, send status, reload and error messages |

## Preview and Send

Moving the cursor shows `METHOD URL`, all headers, and the body in the right pane, and `METHOD URL` in the hint bar. After <kbd>Enter</kbd>, the right pane shows `Sending Resuest` and the hint bar shows `Sending: name` until the response arrives; see [Response Display](/response-display) for the response layout.
