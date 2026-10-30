# 鍵盤操作

本頁列出 TUI 的所有按鍵與焦點（Focus）切換規則。

## 按鍵表

| 按鍵 | 作用範圍 | 功能 |
|------|----------|------|
| <kbd>↑</kbd> <kbd>↓</kbd> | 請求清單 | 移動游標，右欄即時預覽該請求 |
| <kbd>Enter</kbd> | 請求清單 | 送出選取的請求，焦點移至資訊面板 |
| <kbd>↑</kbd> <kbd>↓</kbd> | 資訊面板 | 捲動回應內容 |
| <kbd>Tab</kbd> | 全域 | 在請求清單與資訊面板間切換焦點 |
| <kbd>←</kbd> | 全域 | 焦點移至請求清單 |
| <kbd>→</kbd> | 全域 | 焦點移至資訊面板 |
| <kbd>Ctrl+C</kbd> <kbd>Esc</kbd> | 全域 | 結束程式 |

<kbd>Tab</kbd>、<kbd>←</kbd>、<kbd>→</kbd>、<kbd>Ctrl+C</kbd>、<kbd>Esc</kbd> 由 `App.SetInputCapture` 全域攔截，不會傳到各元件；其餘按鍵交由取得焦點的 `tview` 元件處理。

## 版面

| 區域 | 寬度／高度 | 內容 |
|------|------------|------|
| `API`（左） | 1/3 寬 | 請求名稱清單，選取列以深青色（`tcell.ColorDarkCyan`）整列標示 |
| `Info`（右） | 2/3 寬 | 請求預覽或回應內容，可捲動、自動換行 |
| 提示列（底） | 1 行 | 目前請求、送出狀態、重載與錯誤訊息 |

## 預覽與送出

游標移動時右欄顯示 `METHOD URL`、所有 Header 與 Body，提示列同步顯示 `METHOD URL`。按 <kbd>Enter</kbd> 後右欄先顯示 `Sending Resuest`、提示列顯示 `Sending: 名稱`，回應抵達後才被取代；回應內容說明見 [回應顯示](/zh/response-display)。
