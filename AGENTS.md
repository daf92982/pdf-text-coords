# AGENTS.md — PDF 文字座標擷取器

## 專案目的
純前端工具：使用者選一份 PDF，網頁列出每段文字的內容與文字框四個角落座標，
並在頁面預覽上畫出文字框；可匯出 JSON / CSV。部署於 GitHub Pages。

## 技術限制（不可違反）
- 純靜態：只用 HTML / CSS / JavaScript，不引入打包工具、框架或後端。
- PDF 解析只用 pdf.js，固定版本 3.11.174，檔案放在 `libs/`，不得改回 CDN 或升級版本。
- 必須可離線使用：不得載入任何外部網路資源（字型、圖示、腳本都不行）。
- PDF 檔案只在瀏覽器內處理，不得上傳到任何伺服器。
- 文字必須用 `textContent` 寫入 DOM，禁止用 `innerHTML` 插入 PDF 內容（避免 XSS）。

## 檔案結構與命名
```
index.html        主工具（目前單檔，CSS/JS 內嵌）
libs/             pdf.min.js、pdf.worker.min.js（pdf.js 3.11.174，需一併上傳 GitHub）
AGENTS.md         本規則檔
README.md         專案說明與使用方式
samples/          測試用 PDF，檔名 kebab-case，例如 two-column-zh.pdf
docs/             之後新增的說明頁，檔名 kebab-case.html
```
新頁面：複製 `index.html` 的 `<head>` 與 CSS 變數，放在 `docs/`，並在 README 加連結。

## 座標規則（修改程式前必讀）
- 單位為 PDF point（1/72 吋）。
- 每個文字項目輸出四角：`bl`（左下）、`br`（右下）、`tr`（右上）、`tl`（左上），順序固定。
- 角點由 `item.transform` 的旋轉角、`item.width`、`item.height` 計算，需支援旋轉文字。
- 預設原點為左下（PDF 原生）；選「左上」時 y 一律為 `頁高 - y`。
- 數值四捨五入到小數點後 2 位。
- 欄位名稱固定：`index, page, text, bottom_left, bottom_right, top_right, top_left`；
  CSV 欄位：`index,page,text,bl_x,bl_y,br_x,br_y,tr_x,tr_y,tl_x,tl_y`。改名需同步更新 README。

## 介面風格
- 淺色／深色模式皆可用，顏色只透過 `:root` 的 CSS 變數；強調色只有一個（`--acc`）。
- 介面文字用繁體中文、句首大寫風格的短句、按鈕用動詞（「選擇 PDF」「下載 JSON」）。
- 版面：左側 PDF 預覽、右側座標表；寬度 ≤ 900px 時上下堆疊。
- 預覽與表格需雙向連動（點文字框 ↔ 點表格列）。

## AI 修改時的規則
1. 先讀本檔與 `index.html`，再動手；一次只改一個功能。
2. 不新增依賴；若真的需要，先在 README 說明理由。
3. 不改變上述輸出欄位與順序，除非使用者明確要求。
4. 改完請用 `samples/` 內至少一份 PDF 手動驗證：文字框是否貼齊、旋轉文字是否正確、兩種原點切換是否正確。
5. Commit 訊息用簡短中文，例如「新增 CSV 匯出」。

## 已知限制
- 掃描檔（圖片 PDF）沒有文字層，不會有輸出。
- 頁面本身有 `/Rotate` 時，座標仍以未旋轉頁面計算。
- 文字框高度取字型高度，不含行距。
