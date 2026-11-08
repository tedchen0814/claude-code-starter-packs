---
name: cc-draw
description: Claude Code 安裝 gpt-image-2 生圖 skill。說「安裝生圖」「畫圖」「生圖」時載入。
---

# 生圖技能安裝（Claude Code 版）

## 前置需求
1. OpenAI API Key：https://platform.openai.com/api-keys
2. Organization Individual 驗證：platform.openai.com/settings/organization/general
3. Billing 儲值（最低 US$5）：platform.openai.com → Billing

## 安裝

### 1. 安裝 Python openai 套件
```bash
uv run --with openai python -c "import openai; print(openai.__version__)"
```

### 2. 儲存 API Key
```bash
# PowerShell：請使用者自己執行，Claude 只驗證檔案存在與格式，不讀內容
$k = Read-Host "貼上 OpenAI API Key"; Set-Content "$env:USERPROFILE\.openai.env" "OPENAI_API_KEY=$k" -Encoding ascii
```

### 3. 安裝 draw skill
把本懶人包資料夾裡的 `skills/10-draw/` 整個複製成 `~/.claude/skills/draw/`：
```bash
mkdir -p ~/.claude/skills/draw
cp "<懶人包資料夾>/skills/10-draw/draw.py" ~/.claude/skills/draw/
```
再依 `10-安裝gpt-image-2生圖.md` 的內容建立 `~/.claude/skills/draw/SKILL.md`（生圖用的 skill 說明）。
確認 `~/.claude/skills/draw/draw.py` 存在；不要從 OpenCode 的 `~/.config/opencode/skills/` 混用。

### 4. 測試
```bash
uv run --with openai python ~/.claude/skills/draw/draw.py "一隻橘貓坐在窗邊，水彩風格" --name test --quality low
```

## 品質與費用
| 等級 | 適用 |
|------|------|
| low | 99% 情境（預設）|
| medium | low 明顯不夠時 |
| high | 印刷品 |

實際費用以 OpenAI 官方 pricing 為準，不在 skill 裡寫死金額。

安裝後在任何專案對 Claude 說「畫一張 XX」就會自動生圖。

回報：API key 狀態、draw skill 已安裝、測試生圖結果。
