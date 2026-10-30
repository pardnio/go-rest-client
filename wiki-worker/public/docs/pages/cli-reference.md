# CLI Reference

This page lists the `gorc` command syntax, startup checks, and exit codes.

## Syntax

```bash
gorc <file.http>
```

| Argument | Required | Description |
|----------|----------|-------------|
| `<file.http>` | Yes | Request file to parse and watch; only the first argument is read, the rest are ignored |

There are no flags or subcommands, and no environment variables are read.

## Startup Sequence

| Order | Check | stderr on failure | Exit code |
|-------|-------|-------------------|-----------|
| 1 | File argument provided | `Usage: program {file}.http` | `1` |
| 2 | File exists | `file '<path>' does not exist` | `1` |
| 3 | File info readable | `failed to get file '<path>': <err>` | `1` |
| 4 | Create file watcher | `failed to create file watcher: <err>` | No exit here; step 5 then calls `Add` on a nil watcher and the Go runtime panics (`2`) |
| 5 | Watch the file | `failed to watch file '<path>': <err>` | `1` |
| 6 | Initial parse | `failed to parse file '<path>': <err>` | `1` |
| 7 | Run the TUI | `failed to run TUI: <err>` | `1` |

When all checks pass the TUI starts; exiting normally with <kbd>Esc</kbd> or <kbd>Ctrl+C</kbd> returns `0`.

## Binary Name

| Install method | Binary |
|----------------|--------|
| `go build -o gorc ./cmd/tui` | `gorc` |
| `go install .../cmd/tui@latest` | `$(go env GOPATH)/bin/tui` |

See [Getting Started](/getting-started) for installation steps.
