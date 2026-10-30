# 內部套件

本頁記錄 `internal/ui` 與 `internal/parser` 的匯出型別與函式，供修改或擴充原始碼時參考；兩者位於 `internal/`，無法從其他 module import。

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

| 欄位 | 說明 |
|------|------|
| `App` | `tview` 應用程式；背景 goroutine 的回應渲染與提示列更新經由 `App.QueueUpdateDraw` |
| `Pages` | 建立後未加入版面，目前未使用 |
| `LeftView` | 請求清單（`API` 面板） |
| `RightView` | 請求預覽與回應（`Info` 面板） |
| `HintView` | 底部一行提示列 |
| `Watcher` | 由 `main` 指派的 `fsnotify` 監控器 |
| `Filepath` | 目前載入的 `.http` 路徑，由 `parser.ReadFile` 設定 |
| `Requests` | 解析結果；讀寫須持有 `Mu` |
| `Mu` | 保護 `Requests` 的讀寫鎖 |

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

一個 `.http` 請求區塊的解析結果；`Headers` 同名 Key 以最後一筆為準。

### 函式與方法

| 符號 | 簽章 | 說明 |
|------|------|------|
| `NewTUI` | `func NewTUI() *TUI` | 建立雙欄版面與提示列，註冊全域按鍵（<kbd>Tab</kbd>、<kbd>←</kbd>、<kbd>→</kbd>、<kbd>Ctrl+C</kbd>、<kbd>Esc</kbd>） |
| `UpdateLeftView` | `func (t *TUI) UpdateLeftView()` | 以 `Requests` 重建清單、綁定 <kbd>Enter</kbd> 送出與游標預覽，並預覽第一個請求 |

## internal/parser

| 符號 | 簽章 | 說明 |
|------|------|------|
| `ReadFile` | `func ReadFile(t *ui.TUI, path string) ([]*ui.Request, error)` | 設定 `t.Filepath`、解析整個檔案、在 `Mu` 保護下寫入 `t.Requests`，並於呼叫端 goroutine 呼叫 `UpdateLeftView`；開檔或讀取失敗回傳包裝後的錯誤，`t.Requests` 不變 |
| `WatchFile` | `func WatchFile(t *ui.TUI)` | 讀取 `t.Watcher` 的事件與錯誤直到 channel 關閉；`fsnotify.Write`／`fsnotify.Create` 以 200ms 去抖動後呼叫 `ReloadFile` |
| `ReloadFile` | `func ReloadFile(t *ui.TUI)` | 顯示 `Edited at`，以 `ReadFile` 重新解析 `t.Filepath`，再於事件迴圈內把游標設為清單目前索引（重建後為 0）並顯示 `Reloaded at`；失敗時以紅字顯示錯誤 |

語法細節見 [.http 檔案格式](/zh/http-file-format)，重載行為見 [存檔即時重載](/zh/hot-reload)。
