---
name: cc-obsidian
description: Claude Code 連接 Obsidian MCPVault。說「連接 Obsidian」時載入。
---

# 連接 Obsidian（Claude Code 版）

## 先確認

1. 找 vault 路徑（搜尋含 `.obsidian` 子資料夾的目錄）；列出候選**請使用者確認**，不要自己挑。
2. 問使用者：這個 vault 有沒有不想讓 AI 讀取的私密資料夾？有的話，先把規則寫進 vault 根目錄 `CLAUDE.md`（例如「不讀取、不搜尋、不引用 `<私密資料夾>/`，也不要透過任何工具對它做列出、讀取、寫入、移動、刪除」）。這是規則，不是技術鎖，請如實告知使用者。

## 用 MCP 連接 vault

> Windows 上 Claude Code 用 `npx` 當 MCP 啟動指令時，常因 PATH 問題找不到；本 repo 實測可行的做法是「全域安裝並鎖定版本，設定檔用完整路徑」。細節見根目錄完整版說明（`03-建立第二大腦-Obsidian.md`）。

1. 先說明這會在電腦上安裝一個全域工具，經使用者同意後安裝並**鎖定版本**：`npm install -g @bitbonsai/mcpvault@0.15.0`（Windows 在 Git Bash 需先把 Node.js 加進 PATH）。要升級時先告知使用者，不要自動用 `@latest`。
2. 用 `where.exe mcpvault`（macOS／Linux 用 `which mcpvault`）確認實際路徑。
3. 改設定檔之前，先備份 `~/.claude.json` → `~/.claude.json.bak-<日期時間>`。
4. 加入設定，**預設只裝在目前專案**（不加 `--scope`）：
   - Windows：`claude mcp add obsidian -- "<mcpvault.cmd 完整路徑>" "<VAULT_PATH>"`
   - macOS／Linux：`claude mcp add obsidian -- mcpvault "<VAULT_PATH>"`
   - 沒有 `claude` 指令（桌面版常見）時，改在專案資料夾建立 `.mcp.json`：`{"mcpServers": {"obsidian": {"command": "<完整路徑>", "args": ["<VAULT_PATH>"]}}}`；JSON 內反斜線要寫成 `\\`。
   - 使用者要在**所有專案**都用時，才加 `--scope user`（或改 `~/.claude.json`），並先說明「每個對話都會多載入這批工具、增加固定開銷」，取得同意。
5. 完全關閉 Claude Code 再重新開啟，驗證：**只列出 vault 最上層資料夾**，確認讀得到；再做一次讀取與一次寫入測試（寫入前先告知要建立哪個測試筆記，測完詢問是否刪除）。
6. 載入失敗時，**把完整錯誤訊息貼給使用者，不要自己換方案**。

### 進階（需使用者同意）
若需全文檢索：安裝 Obsidian Local REST API plugin + `pip install cli-anything-hub && cli-hub install obsidian`。

回報：vault 路徑、mcpvault 版本、私密資料夾規則是否已寫入、MCP 設定範圍（目前專案／所有專案）、讀取／寫入測試結果。