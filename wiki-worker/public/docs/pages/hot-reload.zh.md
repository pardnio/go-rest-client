# 存檔即時重載

本頁說明 go-rest-client 如何監控 `.http` 檔並在存檔後重新載入請求清單。

## 觸發條件

啟動時以 `fsnotify` 監控命令列指定的檔案。收到 `fsnotify.Write` 或 `fsnotify.Create` 事件後啟動 200ms 計時器；計時期間再有事件就重設計時器，因此編輯器連續寫入只會觸發一次重載。`fsnotify.Remove`、`fsnotify.Rename`、`fsnotify.Chmod` 事件不觸發重載。

## 重載流程

| 階段 | 提示列 | 說明 |
|------|--------|------|
| 開始 | 黃色 `Edited at HH:MM:SS` | 計時器到期 |
| 解析 | — | 以 `ReadFile` 重新解析整個檔案 |
| 成功 | 綠色 `Reloaded at HH:MM:SS` | 以新解析結果重建清單，游標停在第一個請求，資訊面板預覽該請求 |
| 失敗 | 紅色 `錯誤訊息 at HH:MM:SS` | 清單與資訊面板維持重載前的內容 |

`ReloadFile` 是在 `ReadFile` 重建清單之後才讀取清單目前的索引，而 `tview` 清單重建時游標會歸零，因此每次重載成功後選取的都是第一個請求。

## 監控錯誤

`fsnotify` 回報的錯誤會以紅色 `Watch error: ...` 顯示在提示列，程式持續執行。
