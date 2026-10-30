# 命令列參考

本頁列出 `gorc` 的命令語法、啟動檢查與結束碼。

## 語法

```bash
gorc <file.http>
```

| 參數 | 必要 | 說明 |
|------|------|------|
| `<file.http>` | 是 | 要解析並監控的請求檔；只讀取第一個參數，其餘忽略 |

沒有任何旗標（Flag）或子命令，也不讀取環境變數。

## 啟動流程

| 順序 | 檢查 | 失敗時 stderr | 結束碼 |
|------|------|---------------|--------|
| 1 | 是否提供檔案參數 | `Usage: program {file}.http` | `1` |
| 2 | 檔案是否存在 | `file '<path>' does not exist` | `1` |
| 3 | 能否取得檔案資訊 | `failed to get file '<path>': <err>` | `1` |
| 4 | 建立檔案監控器 | `failed to create file watcher: <err>` | 此處不結束；第 5 步對 nil 監控器呼叫 `Add`，Go runtime panic（`2`） |
| 5 | 監控該檔案 | `failed to watch file '<path>': <err>` | `1` |
| 6 | 首次解析 | `failed to parse file '<path>': <err>` | `1` |
| 7 | 執行 TUI | `failed to run TUI: <err>` | `1` |

全部通過後進入 TUI；以 <kbd>Esc</kbd> 或 <kbd>Ctrl+C</kbd> 正常離開時結束碼為 `0`。

## 執行檔名稱

| 安裝方式 | 執行檔 |
|----------|--------|
| `go build -o gorc ./cmd/tui` | `gorc` |
| `go install .../cmd/tui@latest` | `$(go env GOPATH)/bin/tui` |

安裝步驟見 [快速開始](/zh/getting-started)。
