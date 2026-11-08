# 懶人包 #10：把 ChatGPT Image 2.0 裝進 Claude Code

> 目標：五分鐘內完成安裝，讓 Claude Code 能夠用一句話生圖
> 費用：第一次需要儲值 OpenAI 最少 US$5；每張圖約 **NT$0.3**（low 品質）

---

## 你會得到什麼

裝好後，在 **任何** 專案資料夾裡對 Claude Code 說：

> 「畫一隻穿西裝的龍蝦」

Claude Code 會自動呼叫 OpenAI 的 **gpt-image-2** 生圖，存到當前資料夾的 `slides/generated/`（或 `./generated/`）裡。

---

## 你需要手動準備的三件事

Claude 沒辦法幫你做這三件事（需要本人身份驗證與付款）。做完之後把剩下的全部交給 Claude。

### ① OpenAI 帳號 + 儲值（US$5 起跳）

1. 到 [platform.openai.com](https://platform.openai.com)，用你的 ChatGPT 帳號登入（**同一個帳號**）
2. 右上角頭像 → **Billing** → **Add to credit balance** → 至少儲值 **US$5**（約 NT$160）
3. 可以綁信用卡或用 PayPal

> **為什麼要儲值？** API 是按張計費，跟 ChatGPT 訂閱分開計算。US$5 足夠畫 500 張以上（low 品質）。

### ② 組織 Individual 驗證

gpt-image-2 需要身份驗證才能用。

1. 到 [platform.openai.com/settings/organization/general](https://platform.openai.com/settings/organization/general)
2. 找到 **Verifications** 區塊，點 **Verify Organization**
3. 上傳身分證正反面 + 手持身分證自拍
4. 等 **15 分鐘** 左右驗證通過（會收到 Email）

> **沒做這步會怎樣？** 生圖時會跳 `403 Organization must be verified` 錯誤。

### ③ 取得 API Key

1. 到 [platform.openai.com/api-keys](https://platform.openai.com/api-keys)
2. **Create new secret key** → 名稱填 `claude-code-draw`（隨便取）
3. **Permissions** 選 **All** → **Create**
4. **複製那串 `sk-...` 開頭的 Key**（只會顯示一次，關掉視窗就看不到了）
5. 暫時貼到記事本

---

## 把以下內容整份丟給 Claude Code

> 打開 Claude Code 桌面版（Pro 以上），貼上這整個檔案內容。Claude 會自動執行下列步驟並提示你貼 API Key。

---

# 🤖 Claude 執行區（以下由 AI 自動完成）

你是 Claude Code 助手。請依序完成以下任務。遇到需要使用者提供資訊時，用 AskUserQuestion 詢問。

## 任務 1：確認環境

本懶人包用 `uv` 執行 `draw.py`（不必另外安裝 Python 或用 pip）。檢查：

```bash
uv --version
```

若沒有 uv，先回到 [#00 環境建置](00-環境建置.md) 安裝。

> Windows + Claude 桌面版：`uv run` 會把 Python 與暫存放在 `%APPDATA%`（會被 MSIX 導向），一般用途沒問題；若遇到 `uv trampoline` 或 Python 捷徑錯誤，處理方式見 #00。

## 任務 2：確認 openai 套件可用

不要用 `pip install`（macOS/Linux 的系統 Python 還會要求 `--break-system-packages`，不建議）。改用 uv 臨時帶入套件：

```bash
uv run --with openai python -c "import openai; print(openai.__version__)"
```

之後執行 `draw.py` 也都用 `uv run --with openai python ...`。

## 任務 3：儲存 API Key（金鑰不進對話）

> 🔒 **不要請使用者把 API Key 貼進對話。** 金鑰一旦進入對話就可能被記錄。請使用者自己在終端機輸入，Claude 只驗證檔案存在與格式，不讀內容。

請使用者在自己的終端機執行（PowerShell）：

```powershell
$k = Read-Host "貼上 OpenAI API Key（sk- 開頭）"; Set-Content -Path "$env:USERPROFILE\.openai.env" -Value "OPENAI_API_KEY=$k" -Encoding ascii
```

macOS / Linux：
```bash
read -s -p "貼上 OpenAI API Key: " k; echo; printf 'OPENAI_API_KEY=%s\n' "$k" > ~/.openai.env; chmod 600 ~/.openai.env
```

Claude 只做格式檢查（存在、以 `OPENAI_API_KEY=sk-` 開頭、無多餘空白），不要印出內容。

檔案位置：Windows `C:/Users/<使用者>/.openai.env`；macOS/Linux `~/.openai.env`（在 home 底下，不會被專案的 git 追蹤）。

## 任務 4：建立全域 draw Skill

建立資料夾：

- Windows：`C:/Users/<使用者>/.claude/skills/draw/`
- macOS/Linux：`~/.claude/skills/draw/`

在這個資料夾裡寫入兩個檔案。

### 4-1：`SKILL.md`

```markdown
---
name: draw
description: OpenAI gpt-image-2 生圖技能（全域可用）。當使用者要求「畫一張」、「生一張圖」、「做一張圖」、「產生圖片」、「畫個封面」、「畫插圖」、「畫示意圖」、「畫分鏡」等任何需要 AI 生成圖像的情境時，請一定要使用此技能。此技能會呼叫本地腳本以 gpt-image-2 模型生圖，自動判斷 quality 等級（預設 low），存檔至當前專案的 slides/generated/ 目錄（若無則建於當下工作目錄）。
---

# 小克生圖技能（gpt-image-2）

## 觸發情境
使用者說出：
- 「畫一張 XX」「生一張圖」「做一張圖」
- 「畫個封面／插圖／示意圖／分鏡」
- 「產生圖片」「幫我生圖」

## 腳本位置
- Windows：`C:/Users/<使用者>/.claude/skills/draw/draw.py`
- macOS/Linux：`~/.claude/skills/draw/draw.py`

## 使用方式
```bash
uv run --with openai python <SKILL路徑>/draw.py "要畫的內容" --name 檔名前綴
```

### 參數
- `prompt`（必填）：要畫什麼
- `--size`：`1536x1024`（橫，預設）/ `1024x1024`（方）/ `1024x1536`（直）
- `--quality`：`low`（預設，NT$0.3）/ `medium`（NT$1.3）/ `high`（NT$5.5）
- `--name`：檔名前綴

## 判斷 quality 等級的原則
**預設永遠用 `low`**（省錢 + 速度優先）

- **low**（NT$0.3）：**99% 情境**。演講簡報、技術插圖、封面、demo 都夠。
- **medium**（NT$1.3）：通常不用。
- **high**（NT$5.5）：實體印刷、跨語言文字零錯才用。

不確定就 **low**，不要自作主張升級。

## 錯誤處理
- `403 Organization must be verified` → 到 platform.openai.com/settings/organization/general 做 Individual 驗證
- `401 Invalid API key` → 檢查 `~/.openai.env`
- `429 Rate limit` → 額度用完，到 Billing 儲值

## 輸出
PNG 檔，格式：`<name>_<YYYYMMDD_HHMMSS>.png`
```

### 4-2：`draw.py`

直接複製本懶人包附的腳本，不要自己重寫：

```bash
cp "<懶人包資料夾>/skills/10-draw/draw.py" ~/.claude/skills/draw/draw.py
```

> 腳本只支援 `prompt`、`--name`、`--size`、`--quality`（預設 size `1536x1024`、quality `low`），**不支援**一次多張、改圖或遮罩。

## 任務 5：驗證安裝

測試生成一張圖：

```bash
uv run --with openai python <SKILL路徑>/draw.py "一隻可愛的黑貓，扁平插畫風格" --name test
```

預期結果：
- 終端機顯示 `[OK] .../generated/test_<時間戳>.png`
- 開啟該檔案能看到黑貓圖

若遇到錯誤：
- `403 Organization must be verified` → 使用者沒做 Individual 驗證，引導他到 platform.openai.com/settings/organization/general
- `401 Invalid API key` → 檢查 `~/.openai.env` 裡的 Key 有沒有寫對
- `429 Rate limit` → 使用者沒儲值，提示到 Billing

## 任務 6：回報結果

驗證成功後，告訴使用者：

> 🎉 安裝完成！
>
> 以後在任何專案裡，對我說「畫一張 XX」就能生圖。
>
> - 預設存在 `slides/generated/` 或 `./generated/`
> - 實際費用以 OpenAI 官方 pricing 為準；預設使用 low 品質控制成本
> - 生圖模型：gpt-image-2（OpenAI 最新）

---

# 📚 原理延伸（有興趣再看）

## 為什麼用 Skill 而不是 MCP？

| 方式 | 優點 | 缺點 |
|------|------|------|
| **Skill（本篇）** | 零安裝門檻、按需呼叫、不佔資源、整合費用保護邏輯 | 只做文字生圖，不支援改圖與局部修改 |
| MCP Server | 支援 inpainting、多輪對話 | 要裝 Node.js、改 MCP 設定、常駐 |

對一般使用者來說，**Skill 方式門檻最低**，；本腳本只做「文字生圖」，要改圖或局部修改請另用 MCP 方案。

## 安全性

- API Key 存在 `~/.openai.env`，跟專案 repo 完全分離
- 不會被 git 追蹤
- 所有生圖都在本機執行，不經過第三方伺服器

## 成本控制

三個防護機制內建在 `draw.py`：

1. **預設 low 品質**：一張 NT$0.3
2. **SKILL.md 規則**：Claude 不會自作主張升級 quality
3. **透明計費**：你可以到 [platform.openai.com/usage](https://platform.openai.com/usage) 隨時查用量

---

# 🔗 相關資源

- **OpenAI API 文件**：https://platform.openai.com/docs/guides/images
- **gpt-image-2 模型介紹**：https://platform.openai.com/docs/models/gpt-image-2
- **相關懶人包**：
  - [00 環境建置](00-環境建置.md)（Python 等基礎工具）

---

**最後更新**：2026-04-23
