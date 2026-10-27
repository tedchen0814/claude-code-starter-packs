---
name: cc-vercel
description: Claude Code 串接 Vercel（CLI + MCP + 部署）。說「串接 Vercel」「連接 Vercel」「部署到 Vercel」時載入。
---

# 串接 Vercel（Claude Code 版）

1. 檢查 `node --version`；缺少就先說明並取得同意後再裝 Node.js LTS。
2. 安裝 CLI：`npm i -g vercel@latest` → `vercel --version`（Windows 若被擋就用 `vercel.cmd --version`；公司或群組原則常擋未簽章的 `.ps1`）。已安裝就不要重裝。
3. 登入：`vercel whoami`，未登入就請使用者自己在終端機執行 `vercel login`（瀏覽器授權）；沒帳號請使用者到 https://vercel.com/signup 選 Hobby，建議用 GitHub 註冊。
4. **官方 MCP 為選用，預設不加。** CLI 已登入就能列專案、部署、看紀錄，大多數人不需要 MCP；MCP 會增加每個對話的固定開銷，授權範圍也涵蓋帳號下所有專案。只有使用者明確要求，才說明這些影響並取得同意後再加：`claude mcp add --transport http vercel https://mcp.vercel.com`（預設只裝在目前專案；要所有專案共用才加 `--scope user`）→ 完全關閉 Claude Code 再重開 → 輸入 `/mcp` 選 vercel 完成授權。
5. 測試：先說明會真的上線一個網頁，經同意後才做：在**不屬於任何 git repo** 的空資料夾建 `vercel-test/index.html` → `vercel --yes`（預覽）→ 使用者確認後才 `vercel --prod --yes`（正式）→ 請使用者開網址確認。專案第一次部署會直接算正式環境；專案預設開啟 Deployment Protection，驗證內容用 `vercel curl <網址>`。
6. 選配：`vercel link --yes` + `vercel git connect` 綁 GitHub repo，push 自動部署；綁定前先確認。
7. 詢問是否刪除測試專案：`vercel project rm vercel-test`；刪除前取得確認。

⚠️ `.vercel/` 不 commit；API Key 用 `vercel env add`，不寫進 repo 或對話。

回報：Vercel CLI 版本、登入帳號、MCP 是否有加（預設不加）、測試網址、是否綁定 GitHub。
