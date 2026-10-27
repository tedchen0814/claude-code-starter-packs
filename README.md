# Claude Code 懶人包

> 每個懶人包都是一份 MD 檔，丟給 Claude Code 就能自動完成安裝與設定。個人知識建構與學習使用。

這個 repo 把「讓 Claude Code 變好用」需要的設定拆成一份份懶人包：環境建置、串接 NotebookLM／GitHub／Obsidian／資料庫、本地 AI、免費 API、生圖 Skill，以及開工／收工的工作模式。你不用自己讀文件、敲指令，把懶人包交給 Claude Code，它會照著步驟帶你做，**一次只做一項、做之前先跟你說明**，遇到需要你本人操作（登入、付款、按授權）的地方會停下來告訴你。

---

## 開始之前

- [ ] Claude 帳號（Pro 方案以上）
- [ ] 已安裝 Claude Code（桌面版、CLI 或 IDE 擴充都可以）
- [ ] 電腦有網路連線

貼網址讀檔**不需要先裝 Git**；但開始安裝任何項目之前，Claude 會**先請你安裝 Git**（版本控制工具，後續幾乎都會用到）。其餘缺少的工具（Node.js、Python 等）也會**先跟你說明用途，經你同意才安裝**。

---

## 使用方式

### 方式一：貼網址給 AI（最簡單，不用下載）

開啟 Claude Code，貼上這段話：

```
這是 Claude Code 懶人包全集 https://github.com/tedchen0814/claude-code-starter-packs
請讀取 repo 裡的 SKILL.md，依它的流程帶我安裝：先問我想做什麼，每次只做一項，做之前先跟我說明。
```

Claude 會先問你想做什麼（例如「連接 Obsidian」「把網頁放上網」），提出建議的安裝方案，經你同意後**一次只裝一項**。你不需要先看懂編號；想自己挑時，選「我想自己挑」就會看到完整清單。只給網址時，Claude 會從 `raw.githubusercontent.com` 讀檔，**讀檔不需要先安裝 Git**（安裝項目前會先請你裝 Git）。

> 不建議一次全部安裝：多項會要求登入或修改設定。想先知道目前裝了哪些，請 Claude 做「狀態盤點（00A）」，它只檢查、不改任何東西。

### 方式二：下載到本機再裝

適合網路不穩、或想先看過內容再執行的人。

1. 下載 repo：

   ```bash
   git clone https://github.com/tedchen0814/claude-code-starter-packs.git
   ```

   沒有 Git 也可以在 GitHub 頁面按 **Code → Download ZIP**，解壓縮即可。

2. 在 Claude Code 開啟這個資料夾，貼上：

   ```
   請讀取這個資料夾的 SKILL.md，依它的流程帶我安裝：先問我想做什麼，每次只做一項，做之前先跟我說明。
   ```

### 方式三：只裝某一份

從下面的清單挑一份 MD 檔，把檔案拖進 Claude Code 的對話框（或貼上檔案的 GitHub 網址），說「請照這份懶人包執行，做之前先跟我說明」。

---

## 懶人包清單

| 編號 | 懶人包 | 做什麼 | 安裝時會用到 |
|------|--------|--------|-------------|
| 00 | [環境建置](00-環境建置.md) | 確認並補裝 **Git**、Node.js、uv 等基礎工具（缺的才補，經你同意；Git 是後續所有項目的前置） | — |
| 01 | [連接 NotebookLM](01-連接-NotebookLM.md) | 讓 Claude 讀寫 NotebookLM 筆記本，產生簡報、圖表 | Google 帳號 |
| 02 | [連接 GitHub](02-連接-GitHub.md) | 連接 GitHub，用 GitHub Pages 把網頁上線 | GitHub 帳號 |
| 03 | [建立第二大腦 Obsidian](03-建立第二大腦-Obsidian.md) | 安裝 Obsidian、MCP 連接、Google Drive 同步 | — |
| 04 | [第二大腦設定指南](04-第二大腦設定指南.md) | 三層資料夾結構、CLAUDE.md、筆記模板、每週知識重整 | 先完成 #03 |
| 05 | [連接 Supabase 資料庫](05-連接-Supabase-資料庫.md) | 雲端 SQL 資料庫，讓網頁工具「記得住」資料 | Supabase 帳號 |
| 06 | [連接 Firebase 資料庫](06-連接-Firebase-資料庫.md) | 對新手更友善的資料庫（不會閒置暫停、支援即時更新） | Google 帳號 |
| 07 | [安裝本地 AI Ollama](07-安裝本地AI-Ollama.md) | 在自己電腦跑 AI 模型，免費、離線、資料不外流 | — |
| 08 | [設定 Gemini 免費 API](08-設定Gemini免費API.md) | 申請 Gemini API，讓自製工具有 AI 功能 | Google 帳號 |
| 09 | [初始化研發工具工作模式](09-初始化研發工具工作模式.md) | 建好專案總資料夾、工作筆記、GitHub repo，整理「開工／收工」流程 | 先完成 #02、#03、#06 |
| 10 | [安裝 gpt-image-2 生圖](10-安裝gpt-image-2生圖.md) | 安裝全域 `draw` Skill，之後說「畫一張 XX」就生圖 | OpenAI API Key（需儲值） |
| 11 | [串接 Vercel](11-串接-Vercel.md) | 安裝 Vercel CLI，一句話部署網頁，push 自動更新；私人 repo 也能上線（官方 MCP 為選用） | Vercel 帳號（建議用 GitHub 註冊） |
| 00A | 狀態盤點 | 只檢查哪些已完成、哪些還沒，並給建議；不安裝、不登入 | — |

### 不知道從哪開始？

- **只想先體驗**：#00 → #01
- **想累積自己的知識庫**：#00 → #03 → #04
- **想自己做小工具並上線**：#00 → #02 → #06 → #09（需要私人 repo 或框架專案時再加 #11 Vercel）
- **想知道自己電腦現在裝到哪**：說「做狀態盤點」（00A）

---

## Repo 結構

```
claude-code-starter-packs/
├── 00-環境建置.md … 11-串接-Vercel.md             ← 完整版懶人包（含說明、步驟、FAQ）
├── SKILL.md                                      ← 給 AI 讀的安裝入口（目標選單、通用規則、編號對照）
├── skills/<編號-名稱>/SKILL.md                    ← 每份懶人包的精簡版步驟（00-install-all 為狀態盤點）
└── workflow-skills/                               ← #09 會安裝的三個技能：project-init-sync、startup-sync、shutdown-sync
```

- 想了解原理、自己看步驟：讀根目錄的完整版 MD 檔
- 交給 AI 安裝：AI 會從 `SKILL.md` 進入，依序讀 `skills/` 裡的精簡版步驟
- 檔名前面的數字與上表編號一致

---

## 常見問題

| 問題 | 解法 |
|------|------|
| 安裝到一半中斷了 | 重新跑同一份懶人包即可，每一步都會先檢查是否已完成 |
| 裝完 MCP，Claude 卻說找不到工具 | 完全關閉 Claude Code 再重新開啟 |
| 某一步一直失敗 | 把錯誤訊息貼給 Claude，請它對照該懶人包的 FAQ 排除 |
| 換了一台電腦 | 在新電腦重跑需要的懶人包 |
| 想還原設定 | 改 `~/.claude.json` 之前都會先備份成 `.claude.json.bak-<日期時間>`（同一個資料夾），把備份檔複製回原檔名即可 |
| 想移除某個 MCP | `claude mcp remove <名稱>`（若當初用 `--scope user` 加入，要加 `--scope user`）；沒有 `claude` 指令時，手動刪掉專案 `.mcp.json` 或 `~/.claude.json` 裡的對應項目 |
| 想移除某個技能 | 刪掉 `~/.claude/skills/` 底下對應的資料夾，再完全關閉 Claude Code 重開 |
| 讀 raw 檔案中文變亂碼（Windows PowerShell 5.1） | 改用 UTF-8 解碼重抓；不要根據亂碼的內容行動 |
| 直接打 `vercel`、`firebase` 指令失敗（Windows） | 改打 `vercel.cmd`、`firebase.cmd`；公司或群組原則常擋未簽章的 `.ps1` |

---

## ⚠️ 使用提醒

- 個資與機敏資料不要放進公開的 GitHub repo 或免費雲端資料庫
- API Key、Token 只存在本機，不要寫進 repo，也不要貼進對話
- MCP 預設只裝在**目前專案**；每個 MCP 都會讓每個對話多載入一批工具、增加固定開銷，裝成所有專案共用前請先確認真的需要
- 免費方案的額度與規則可能變動，以各服務官網為準

---

## 授權

MIT License，詳見 [LICENSE](LICENSE)。
