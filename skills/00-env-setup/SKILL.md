---
name: cc-env-setup
description: Claude Code 環境建置（Git, Node.js, uv）。說「建置環境」「安裝開發環境」時載入。
---

# Claude Code 環境建置

## 步驟

1. 先檢查（唯讀）：`git --version`、`node --version`、`uv --version`。**Git 是後續所有項目的前置條件，一定要有**：沒有就先說明用途（版本控制工具）並經同意安裝，使用者不同意就停在這裡，不繼續其他項目。
2. 缺少的才補。**先說明要裝什麼、為什麼需要、是否需要系統管理員權限，經使用者同意再裝**；已有的不要重裝。
3. 裝完開**新的終端機視窗**再檢查版本（舊視窗讀不到新的 PATH）。
4. 最終驗證所有版本。

| 工具 | Windows | macOS | Linux |
|------|---------|-------|-------|
| Git | `winget install --id Git.Git -e`（可能跳出系統管理員確認，要使用者自己按） | `brew install git` | 依發行版的套件管理員安裝（如 `sudo apt install git`） |
| Node.js（要 LTS 版） | `winget install --id OpenJS.NodeJS.LTS -e` | `brew install node` | 依發行版的套件管理員安裝 Node.js LTS（如 `sudo apt install nodejs npm`，版本需 18 以上） |
| uv | `winget install --id astral-sh.uv -e` | `brew install uv` | `curl -LsSf https://astral.sh/uv/install.sh \| sh` |

- Windows 沒有 winget 時，請使用者從官網下載安裝，不要自己找替代來源。
- 在 Windows，npm 全域工具（如 `vercel`、`firebase`）若直接打指令失敗，改用 `.cmd` 版（如 `vercel.cmd`）；公司或群組原則常擋未簽章的 `.ps1`。
- 要從網路下載並直接執行的安裝腳本（`curl … | sh`），先把網址與用途告訴使用者，經同意再執行。

回報格式：已安裝／已補裝清單＋版本號（用白話）。
