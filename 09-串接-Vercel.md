---
title: 'Claude Code 懶人包 #09：串接 Vercel'
date: '2026-10-26'
type: 懶人包
version: v0.1
status: 初版（實作後更新）
tags:
  - Claude-Code
  - 懶人包
  - Vercel
  - MCP
  - 部署
---
# Claude Code 懶人包 #09：串接 Vercel

> 版本：v0.1
> 更新日期：2026-10-26

> 📌 **本懶人包可獨立執行**：會自動檢查並安裝所需工具，不需要先看過其他懶人包。你只要確認下方「先備條件」即可開始。

---

## 這個懶人包會幫你做什麼？

讓你的 Claude Code 能夠直接操作 Vercel，包括：
- 安裝 Vercel CLI，一句話就把網頁部署上線，拿到 `https://xxx.vercel.app` 網址
- 連接 Vercel MCP，讓 Claude 直接查部署狀態、看建置 log、找錯誤
- （選配）綁定 GitHub repo，之後每次 `git push` 自動重新部署
- 用一個測試網頁走完整個流程，確認一切正常

### Vercel 和 GitHub Pages 怎麼選？

| 項目 | GitHub Pages（懶人包 #02） | Vercel（本懶人包） |
|------|---------------------------|-------------------|
| 適合 | 純 HTML／CSS／JS 靜態網頁 | 靜態網頁，以及 Next.js、React 等需要建置的專案 |
| repo 公開／私人 | 免費版需公開 repo | 私人 repo 也能部署 |
| 每次改版 | push 後自動更新 | push 後自動更新，每個分支還有預覽網址 |
| 後端功能 | 沒有 | 有（Serverless Functions、環境變數） |
| 出錯時 | 自己看 Actions log | Claude 透過 MCP 直接讀建置 log |

> 💡 簡單的單頁工具用 GitHub Pages 就夠；想放私人 repo、用框架、或需要 API 金鑰藏在後端時，選 Vercel。

---

## 先備條件

在使用這個懶人包之前，請確認：

- [ ] Claude Code 已安裝且能正常使用（Pro 方案以上）
- [ ] 有可用的 email 或 GitHub 帳號（如果還沒 Vercel 帳號，懶人包會引導你註冊）
- [ ] 電腦有網路連線

---

## 請 Claude Code 幫我執行以下步驟

> ⚠️ 以下內容是給 Claude Code 讀的操作指令。
> 你只需要把這整份 MD 檔丟給 Claude Code，它會自動開始執行。
> 遇到需要你手動操作的地方，它會暫停並告訴你該怎麼做。

---

### 步驟零：環境檢查

> 請 Claude 在開始前，先自動確認以下所有項目。
> 如果有任何一項不符合，請先告知使用者問題所在，並引導解決後再繼續。
> **不要跳過任何一項檢查，不要假設環境正常。**

1. **確認作業系統**：執行系統指令確認是 Windows / macOS / Linux，後續指令依實際作業系統選擇
2. **確認網路連線正常**
3. **檢查 Node.js 是否已安裝**：執行 `node --version` 與 `npm --version`，如果未安裝：
   - Windows：`winget install OpenJS.NodeJS.LTS --accept-source-agreements --accept-package-agreements`
   - macOS：`brew install node`
   - Linux：`curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash - && sudo apt install nodejs -y`
4. **檢查 Vercel CLI 是否已安裝**：執行 `vercel --version`（未安裝的話步驟一會處理）
5. **檢查 Claude Code CLI 是否可用**：執行 `claude --version`（步驟三註冊 MCP 會用到）

> 全部通過後，告知使用者：「環境檢查完成，開始設定。」
> 安裝完工具後如果指令找不到，請提醒使用者可能需要重啟 Claude Code。

---

### 步驟零.五：確認使用者有 Vercel 帳號（如果還沒有，幫忙註冊）

> 🖐️ **需要手動操作**：請 Claude **先詢問使用者**：「你有 Vercel 帳號了嗎？」
>
> - **已有帳號** → 跳到步驟一
> - **還沒有** → 依照以下流程引導使用者註冊（約 3 分鐘）

#### 註冊 Vercel 帳號引導流程

1. 請使用者在瀏覽器開啟：https://vercel.com/signup
2. 方案選 **Hobby**（免費，限個人、非商業用途）
3. 註冊方式**建議選「Continue with GitHub」**：之後綁定 GitHub repo 自動部署最方便。沒有 GitHub 帳號也可以用 email 註冊
4. 依畫面完成驗證，看到 Vercel Dashboard 就代表成功

> 告知使用者：「✅ Vercel 帳號註冊完成！接下來幫你連接 Claude Code。」

---

### 步驟一：安裝 Vercel CLI

```bash
npm i -g vercel@latest
vercel --version
```

> 若 Windows 出現權限錯誤，改用系統管理員身分開啟 Claude Code 再試，或確認 npm 全域路徑已加入 PATH。

---

### 步驟二：登入 Vercel

檢查是否已登入：
```bash
vercel whoami
```

如果尚未登入：
```bash
vercel login
```

> 🖐️ **需要手動操作**：
> 1. 終端機會開啟瀏覽器（或顯示一個網址與驗證碼）
> 2. 請使用者在瀏覽器確認登入並授權
> 3. 完成後回到 Claude Code

登入成功後再跑一次 `vercel whoami`，確認顯示使用者名稱。

---

### 步驟三：連接 Vercel MCP

Vercel 官方提供遠端 MCP 伺服器 `https://mcp.vercel.com`，讓 Claude 可以直接查專案、部署與 log。

註冊到使用者層級（所有專案都能用）：
```bash
claude mcp add --transport http --scope user vercel https://mcp.vercel.com
```

確認已加入：
```bash
claude mcp list
```

> 🖐️ **需要手動操作：授權 MCP**
> 1. 完全關閉 Claude Code 再重新開啟
> 2. **CLI 版**：在對話中輸入 `/mcp`，選 `vercel` → Authenticate，瀏覽器會開啟 Vercel 授權頁，按允許
> 3. **桌面版**：若出現 vercel 需要驗證的提示，依指示在瀏覽器授權；如果沒有提示，開一個終端機執行 `claude`，輸入 `/mcp` 完成授權即可（授權結果桌面版共用）
> 4. 授權完成後回到對話

驗證：請 Claude 說「列出我的 Vercel 專案」。能回傳清單（新帳號可能是空的）就代表 MCP 正常。

> 💡 **選配：Vercel 官方 Plugin**。Vercel 另外提供給 Claude Code 用的 Plugin（含部署等指令），有興趣可執行 `npx plugins add vercel/vercel-plugin`。不裝也不影響本懶人包。

---

### 步驟四：部署第一個測試網頁

1. 在使用者的文件資料夾下建立臨時資料夾 `vercel-test`
2. 建立 `index.html`，內容為「Hello！Vercel 連接成功！」
3. 在 `vercel-test` 資料夾內執行預覽部署：

```bash
vercel --yes
```

> `--yes` 會用預設值建立專案（專案名稱 `vercel-test`）。完成後終端機會顯示一個 **Preview** 網址。

4. 再部署到正式環境：

```bash
vercel --prod --yes
```

5. 把 **Production** 網址（`https://vercel-test-xxx.vercel.app` 或 `https://vercel-test.vercel.app`）告知使用者

> 🖐️ **需要手動操作**：請使用者開啟瀏覽器，確認網頁顯示「Hello！Vercel 連接成功！」

6. 用 MCP 驗證：請 Claude 透過 Vercel MCP 查詢 `vercel-test` 最新一次部署的狀態，應為 `READY`

> 📌 `vercel` 會在資料夾內建立 `.vercel/`（記錄專案連結），**不要 commit 進 git**。若資料夾是 git repo，確認 `.gitignore` 有 `.vercel`。

---

### 步驟五（選配）：綁定 GitHub repo，push 自動部署

> 🖐️ **詢問使用者**：「要不要讓某個 GitHub repo 在每次 push 後自動部署到 Vercel？」不要就跳到步驟六。

**做法 A：在 Claude Code 裡完成**（專案資料夾已有 GitHub remote 時）
```bash
cd <專案資料夾>
vercel link --yes
vercel git connect
```

**做法 B：用網頁**
1. 開啟 https://vercel.com/new
2. 選 **Import Git Repository**，第一次需授權 Vercel 讀取 GitHub（可只授權指定的 repo）
3. 選好 repo → Deploy

綁定後：
- push 到 `main` → 自動更新正式網址
- push 到其他分支 → 產生該分支的預覽網址

---

### 步驟六：清除測試專案（可選）

> 🖐️ **詢問使用者**：「測試成功了！要保留 `vercel-test` 專案還是刪除？」

如果要刪除（會詢問確認，請使用者同意後再回答 yes）：
```bash
vercel project rm vercel-test
```

並刪除本地的 `vercel-test` 資料夾。

告知使用者：「✅ 全部完成！你的 Claude Code 已成功串接 Vercel。以後只要說『幫我部署到 Vercel』，Claude 就會自動處理。」

---

## 完成！接下來你可以這樣用

| 你說的話 | Claude + Vercel 會做的事 |
|----------|------------------------|
| 「幫我把這個資料夾部署到 Vercel」 | `vercel --prod` → 給你正式網址 |
| 「先給我一個預覽網址看看」 | `vercel` 預覽部署，不影響正式網址 |
| 「上次部署為什麼失敗？」 | 透過 MCP 讀建置 log，找出錯誤並修正 |
| 「列出我所有 Vercel 專案」 | 透過 MCP 查詢專案清單 |
| 「幫我把 API Key 設成環境變數」 | `vercel env add`，金鑰不進 repo |
| 「回到上一版」 | `vercel rollback` |

---

## 如果安裝失敗，如何重來

對 Claude Code 說：
「Vercel 懶人包執行失敗了，幫我檢查哪裡出問題，重新處理。」

Claude 會自動：
1. 重新執行環境檢查
2. 確認 Vercel CLI 與登入狀態（`vercel whoami`）
3. 確認 MCP 設定（`claude mcp list`）
4. 找出問題並修復

完全重置：
```bash
vercel logout
claude mcp remove vercel --scope user
```
然後從步驟一重來。

---

## 常見問題

| 問題 | 解法 |
|------|------|
| `vercel: command not found` | 重啟 Claude Code；或確認 `npm config get prefix` 的路徑已加入 PATH |
| `vercel login` 瀏覽器沒開啟 | 複製終端機顯示的網址，手動在瀏覽器開啟 |
| Claude 說找不到 Vercel 工具 | 完全關閉 Claude Code 再開；確認 `claude mcp list` 有 vercel 且已完成授權 |
| MCP 顯示需要驗證（needs auth） | 在 CLI 版輸入 `/mcp` → vercel → Authenticate |
| 部署後網頁 404 | 確認 `index.html` 在部署資料夾的根目錄 |
| 建置失敗（Build Failed） | 請 Claude「用 MCP 讀最新一次部署的 build log」，依錯誤修正 |
| （實作後持續補充） | |

---

## 免費方案說明（Hobby）

- 限**個人、非商業**用途，適合學習與個人作品
- 額度（流量、建置時間、函式執行等）以 Vercel 官網 Pricing 頁為準，超出時服務會受限，不會自動扣款
- 私人 repo 可以部署

---

## 安全提醒

- API Key、Token 用 `vercel env add` 設成環境變數，不要寫在 HTML 或 commit 進 repo
- `.vercel/` 資料夾不要 commit
- 部署出去的網址任何人都能開啟，不要放個資與機敏資料
- 綁定 GitHub 時，可以只授權需要的 repo，不必授權全部

---

## 更新紀錄

| 日期 | 版本 | 更新內容 |
|------|------|---------|
| 2026-10-26 | v0.1 | 初版：Vercel CLI、登入、Vercel MCP、測試部署、GitHub 自動部署 |

---

## 相關連結

- [懶人包 #00：環境建置](00-環境建置.md)
- [懶人包 #02：連接 GitHub](02-連接-GitHub.md)
- [Vercel MCP 官方文件](https://vercel.com/docs/agent-resources/vercel-mcp)
- [Vercel CLI 官方文件](https://vercel.com/docs/cli)
- [README｜Claude Code 懶人包索引](README.md)
