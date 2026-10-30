# 回應顯示

本頁說明一般（非串流）HTTP 回應在資訊面板與提示列的呈現方式。

## 版面結構

```
200 OK
Headers:
  Content-Type: application/json; charset=utf-8
  Duration: 245.123ms

Body:
  {
    "login": "pardnchiu"
  }
```

| 區段 | 內容 |
|------|------|
| 狀態行 | `resp.Status`（例：`200 OK`），依狀態碼上色 |
| `Headers:` | 每個回應 Header 一行，多值 Header 拆成多行；順序不固定 |
| `Duration:` | 從送出到讀完 Body 的耗時 |
| `Body:` | 回應 Body，可解析為 JSON 時自動縮排 |

## 狀態碼顏色

| 狀態碼 | 顏色 | 套用位置 |
|--------|------|----------|
| < 300 | 綠 | 狀態行、提示列狀態碼 |
| 300–399 | 黃 | 同上 |
| ≥ 400 | 紅 | 同上，且整段 Body 以紅色顯示 |

## JSON 格式化

Body 能被 `json.Unmarshal` 解析時，以兩格空白重新縮排；解析失敗（HTML、純文字、空 Body）則原樣顯示。格式化只影響顯示，不改變送出或接收的資料。

## 提示列

完成時提示列顯示 `(狀態碼) METHOD URL | 耗時`，例：

```
(200) GET https://api.github.com/users/pardnchiu | 245.123ms
```

請求失敗時的顯示見 [請求錯誤](/zh/response-errors)。
