---
name: cc-supabase
description: Claude Code 連接 Supabase MCP。說「連接 Supabase」時載入。
---

# 連接 Supabase（Claude Code 版）

1. 不做全域安裝（不要 `npm install -g`），改用 `npx` 直接啟動。
2. 請使用者**自己**登入 Supabase，取得 project ref 與 personal access token；token 由使用者在自己的終端機輸入，不要貼進對話、不要寫進 repo。
3. 改設定檔前先備份 `~/.claude.json`，再用 `claude mcp add supabase -- npx -y @supabase/mcp-server-supabase --project-ref <ref> --access-token <token>`（Windows 用 `npx.cmd`）。**預設只裝在目前專案**；要所有專案共用才加 `--scope user`，並先說明「每個對話都會多載入這批工具」，取得同意。
4. 完全關閉 Claude Code 再重開，驗證：只做唯讀查詢（列出資料表），不要建立或修改資料。

⚠️ 不把 API key 寫進 CLAUDE.md 或 repo。