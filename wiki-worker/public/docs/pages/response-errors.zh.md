# 請求錯誤

本頁說明請求無法建立、連線失敗或讀取失敗時，資訊面板與提示列顯示的內容。

## 錯誤類型

| 情況 | 資訊面板 | 提示列 |
|------|----------|--------|
| 無法建立請求（URL 格式錯誤等） | `Error creating request: ...` | 維持 `Sending: 名稱` |
| 連線失敗、逾時 | `Error: ...` | 紅色錯誤訊息 |
| 讀取 Body 失敗 | 狀態與 Header 照常，Body 位置顯示 `failed read body: ...` | 狀態碼與耗時 |

## 逾時

每個請求同時受 120 秒的 `context` 逾時與 `http.Client.Timeout` 限制；在收到回應 Header 前逾時，資訊面板顯示 `Error: ...`；在讀取 Body 期間逾時，則顯示為 `failed read body: ...`。一般回應的格式見 [回應顯示](/zh/response-display)，串流回應見 [SSE 串流](/zh/sse-streaming)。
