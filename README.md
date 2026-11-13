# PDF 文字座標擷取器

在瀏覽器選一份 PDF，列出每段文字與其四個角落座標（左下、右下、右上、左上），可匯出 JSON / CSV。

- 網頁：`https://<你的帳號>.github.io/<repo 名稱>/`
- 規則檔：[AGENTS.md](AGENTS.md)

## 準備 pdf.js（只需做一次）
下載下列兩個檔案，放進專案的 `libs/` 資料夾：
- https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.min.js
- https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.worker.min.js

## 本機執行（可離線）
直接用瀏覽器開 `index.html`，或 `python -m http.server` 後開 http://localhost:8000。

## 部署
GitHub → Settings → Pages → Branch 選 `main`、資料夾 `/ (root)` → Save。
