# 快速開始

本頁說明安裝終端機 API 測試工具 go-rest-client、送出第一個請求，以及它與其他 API 客戶端的差異。

## 前置需求

| 項目 | 需求 |
|------|------|
| Go | 1.25.1 以上（`go.mod` 宣告版本；僅原始碼建置與 `go install` 需要） |
| 終端機 | `tcell` 支援的終端機，由 `tcell` 負責繪製 |
| 網路 | 可連線至 `.http` 檔中的目標 API |

## 安裝

### 從原始碼建置

```bash
git clone https://github.com/pardnchiu/go-rest-client.git
cd go-rest-client
go build -o gorc ./cmd/tui
sudo cp gorc /usr/local/bin/gorc
```

### 使用 go install

```bash
go install github.com/pardnchiu/go-rest-client/cmd/tui@latest
sudo cp "$(go env GOPATH)/bin/tui" /usr/local/bin/gorc
```

`go install` 依 package 目錄命名執行檔為 `tui`；複製為 `gorc` 只是為了好記，本站其餘頁面皆以 `gorc` 稱呼。

## 第一次執行

建立 `api.http`：

```http
### Get User Info
GET https://api.github.com/users/pardnchiu
Accept: application/json

###
```

啟動：

```bash
gorc api.http
```

左欄出現「Get User Info」，右欄預覽請求行與 Header。按 <kbd>Enter</kbd> 送出，右欄改為顯示狀態碼、回應 Header、耗時與格式化後的 JSON Body；按 <kbd>Esc</kbd> 或 <kbd>Ctrl+C</kbd> 離開。

## go-rest-client 的定位

Posting 與 ATAC 是功能完整的終端機 API 客戶端，請求存在各自的集合檔（Posting 用 YAML，ATAC 用 JSON 或 YAML），並提供環境變數與匯入工具。resterm 同樣在終端機執行純文字 `.http` 檔，另外支援 GraphQL、gRPC、WebSocket、測試腳本與 CI 執行器。VS Code 的 REST Client 擴充套件則在編輯器內執行以 `###` 分隔的 `.http` 請求區塊。go-rest-client 維持精簡：單一 Go 執行檔開啟一個 `.http` 檔、列出其中的請求、每次存檔 200ms 後重新解析，並逐行顯示 `text/event-stream` 回應。請求依檔案內容原樣送出，`{{variable}}` 佔位符不會被展開。

## 下一步

- 撰寫更多請求：[.http 檔案格式](/zh/http-file-format)
- 熟悉操作：[鍵盤操作](/zh/keyboard-controls)
- 邊改邊測：[存檔即時重載](/zh/hot-reload)
