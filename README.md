# Claude Code 懶人包

> 每個懶人包都是一份 MD 檔，丟給 Claude Code 桌面版就能自動完成安裝與設定。個人知識建構與學習使用。

---

## 使用方式

### 方式一：直接叫 AI 幫你裝（最簡單）

1. 把這個 repo clone 或下載到自己的電腦
2. 在 Claude Code 桌面版開啟這個資料夾，貼上這段話：

```
這個資料夾是 Claude Code 懶人包全集。
請讀取 SKILL.md，列出所有可用的懶人包，問我要裝哪些。
```

AI 會讀取 `SKILL.md`（安裝入口），列出 12 個技能，讓你選擇後自動安裝。

### 方式二：單獨跑某一份

1. 從清單挑一份懶人包（MD 檔）
2. 打開 Claude Code 桌面版（Pro 以上），把檔案丟給它
3. AI 自動執行，遇到需要手動操作的地方會暫停指示你

---

## 🌟 每份懶人包都是獨立可執行的

每份懶人包都會**自動檢查並安裝**需要的工具（Git、Node.js、uv 等），不需要先執行 #00 環境建置。

---

## 最低先備條件

使用任何懶人包之前，請確認：

- [ ] Claude 帳號已註冊（Pro 方案以上）
- [ ] Claude Code 桌面版已安裝
- [ ] 電腦有網路連線

---

## 懶人包清單

| 編號 | 懶人包名稱 | 狀態 | 說明 |
|------|-----------|------|------|
| 00 | [環境建置](00-環境建置.md) | v0.2 | 安裝 Git、GitHub CLI、uv 等基礎工具 |
| 01 | [連接 NotebookLM](01-連接-NotebookLM.md) | v0.2 | 安裝 NotebookLM MCP + 產生簡報與圖表 |
| 02 | [連接 GitHub](02-連接-GitHub.md) | v0.2 | 連接 GitHub + GitHub Pages 網頁上線 |
| 03 | [建立第二大腦 Obsidian](03-建立第二大腦-Obsidian.md) | v0.5 | 安裝 Obsidian + MCP 連接 + Google Drive 同步 |
| 03+ | [第二大腦設定指南](04-第二大腦設定指南.md) | v1.0 | 三層結構 + CLAUDE.md + 模板 + 每週知識重整排程 |
| 04 | [連接 Supabase 資料庫](04-連接-Supabase-資料庫.md) | v0.2 | 連接雲端資料庫，讓程式「記得住」 |
| 04.5 | [連接 Firebase 資料庫](04.5-連接-Firebase-資料庫.md) | v0.7 | 對新手更友善的資料庫選擇（不會閒置暫停、千人活動撐得住、Firestore MCP 完整 CRUD） |
| 05 | [安裝本地 AI Ollama](05-安裝本地AI-Ollama.md) | v0.2 | 安裝本地 AI，免費、隱私、離線可用 |
| 06 | [設定 Gemini 免費 API](06-設定Gemini免費API.md) | v0.2 | 設定 Gemini 免費 API，不用信用卡 |
| 07 | [初始化研發工具工作模式](07-初始化研發工具工作模式.md) | v0.1 | 一鍵啟動「研發建專案模式」：建好總資料夾、CLAUDE.md、Obsidian 工作筆記、GitHub repo、安裝 `/收工` skill |
| 08 | [把 ChatGPT Image 2.0 裝進 Claude Code](08-安裝gpt-image-2生圖.md) | v0.1 | 全域 `draw` Skill 安裝：OpenAI API Key + Individual 驗證 + `~/.claude/skills/draw/` + 第一張圖驗證。之後在任何專案對 Claude 說「畫一張 XX」就生圖；費用以 OpenAI 官方 pricing 為準 |

---

## ⚠️ 使用提醒

- 個資與機敏資料不要放進公開的 GitHub repo 或免費雲端資料庫
- API Key、Token 只存在本機，不要寫進 repo
