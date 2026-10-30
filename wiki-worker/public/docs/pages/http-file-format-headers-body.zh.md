# Header 與 Body

本頁說明請求行之後的每一行如何被判定為 Header 或 Body。

## Header

請求行之後、進入 Body 之前，含 `:` 的行會以第一個 `:` 拆成 Key 與 Value（兩側去空白）。Key 須通過 `checkHeader`：

| 條件 | 範例 |
|------|------|
| 以英文字母開頭 | `X-Api-Key` 可，`1Key` 不可 |
| 僅含字母、數字、`-`、`_` | `Content_Type` 可，`Content Type` 不可 |
| 不以 `"` 開頭的行 | `"key": "value"` 一律視為 Body |

不含 `:` 或 Key 不合格的行即開始 Body；因此純文字 Body 的第一行若形如 `Key: value`，會被當成 Header。Header 存於 `map`，同名 Key 以最後一行為準。

## Body

Header 與 Body 之間的空行會被略過，不會開始 Body。Body 從第一個符合下列觸發條件的行開始，直到下一個 `###` 或請求行：

| 觸發 | 說明 |
|------|------|
| `{` 或 `[` 單獨成行 | 多行 JSON；該行即為 Body 第一行 |
| 不是合格 Header 的行 | 例如單行 JSON `{"id": 1}` 或表單資料 `name=value`；該行會被加入 Body 兩次，單行 Body 實際送出 `{"id": 1}\n{"id": 1}`。要原樣送出 JSON，請讓 `{` 或 `[` 單獨成行 |

進入 Body 後以原始行（保留縮排）組合，換行為 `\n`；空行與 `//`、`--` 開頭的行視為內容，`#`（但非 `###`）開頭的行則被捨棄。

請求區塊與註解規則見 [.http 檔案格式](/zh/http-file-format)。
