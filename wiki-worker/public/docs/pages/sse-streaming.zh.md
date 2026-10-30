# SSE 串流

本頁說明 Server-Sent Events（SSE）回應如何被偵測並即時顯示。

## 偵測

回應 Header `Content-Type` 包含 `text/event-stream` 時切換為串流模式；判斷只看回應，請求是否帶 Accept: text/event-stream Header 不影響。

```http
### SSE Stream
GET https://sse.dev/test
Accept: text/event-stream

###
```

## 即時渲染

| 行為 | 說明 |
|------|------|
| 逐行附加 | `bufio.Scanner` 每讀到一行就重繪資訊面板，原始行原樣顯示（含 `data:`、`event:` 前綴） |
| 自動捲動 | 每次重繪後捲到最底 |
| 顯示上限 | 只保留最後 100 行，較早的行不再顯示 |
| 標頭列 | 狀態行與 Header 之後顯示 `Duration: 經過時間 \| Events: 數量`，每行更新一次 |

`Events` 為已讀行數除以 2，假設每個事件由一行資料加一個空行組成；多行 `data:` 或帶 `event:`／`id:` 的事件會讓數字與實際事件數不符。

## 結束

伺服器關閉連線後，提示列顯示 `(狀態碼) METHOD URL | 總耗時`；讀取發生錯誤時改顯示紅色 `Stream error: ...`，已收到的內容保留在資訊面板。

## 逾時

HTTP client 的 `Timeout` 固定為 120 秒，且涵蓋讀取 Body 的時間。
