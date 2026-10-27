---
name: cc-firebase
description: Claude Code 連接 Firebase MCP。說「連接 Firebase」時載入。
---

# 連接 Firebase（Claude Code 版）

1. 先檢查 `firebase --version`（Windows 用 `firebase.cmd`）；缺少才說明並經同意後 `npm install -g firebase-tools`，再請使用者自己在終端機執行 `firebase login`。
2. 在專案目錄 `firebase init`（先說明會建立哪些檔案，經同意再做）。
3. 改設定檔前先備份 `~/.claude.json`，再用 `claude mcp add firebase -- npx -y firebase-tools@latest mcp`（Windows 用 `npx.cmd`）。**預設只裝在目前專案**；要所有專案共用才加 `--scope user`，並先說明「每個對話都會多載入這批工具」，取得同意。
4. 完全關閉 Claude Code 再重開，驗證：只列出專案，不要建立或修改。

⚠️ Admin SDK 憑證不可公開，個人資料只存代號。