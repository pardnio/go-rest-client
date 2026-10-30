# .http 檔案格式

本頁說明解析器（Parser）如何切分請求區塊、辨識請求行與略過註解；Header 與 Body 的判定見 [Header 與 Body](/zh/http-file-format-headers-body)。

## 請求區塊

每個請求以單獨一行 `###` 結束並加入清單；`### 名稱` 這一行只定義**下一個**請求在左欄顯示的名稱，**不會**結束目前請求。檔案結尾時未結束的請求也會被收錄。

```http
### Create Post
POST https://jsonplaceholder.typicode.com/posts
Content-Type: application/json

{
  "title": "Test Post",
  "userId": 1
}

###
```

未寫 `### 名稱` 的請求，左欄名稱為空字串。

## 請求行

請求行須符合 `^(GET|POST|PUT|DELETE|PATCH|HEAD|OPTIONS)\s+(.+)$`：

| 規則 | 說明 |
|------|------|
| 方法 | 僅限上述 7 個，且必須大寫；`get` 不會被辨識 |
| URL | 方法後的**整段剩餘文字**；行尾若寫 `HTTP/1.1` 會被併入 URL |
| 位置 | 遇到請求行即開始新請求，Header 與 Body 狀態重設；前一個請求只有在已有 Body 時才會加入清單，沒有 Body 則被捨棄，因此每個請求都以 `###` 結束 |

## 註解

一行是否略過，取決於解析器當下是否在 Body 內：

| 行 | Body 外 | Body 內 |
|----|---------|---------|
| 空行 | 略過 | 保留為 Body 內容 |
| 以 `//` 或 `--` 開頭 | 略過 | 保留為 Body 內容 |
| 以 `#`（但非 `###`）開頭 | 略過 | 略過，不會加入 Body |
| `### 名稱` | 設定下一個請求的名稱 | 設定下一個請求的名稱，不會加入 Body |
