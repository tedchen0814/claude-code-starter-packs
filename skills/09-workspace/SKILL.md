---
name: cc-workspace
description: Claude Code 研發建專案工作模式。說「初始化研發工具」「研發建專案」時載入。
---

# 研發建專案工作模式（Claude Code 版）

## 初始化
1. 問：專案名稱、用途、資料夾；GitHub repo（預設不建，要建一律私有並先同意）、部署需求。已有同名或近似名稱的資料夾時先停下來問，不要覆蓋。
2. 建立：`CLAUDE.md`、`README.md`、`工作筆記.md`、`.gitignore`（至少排除 `.env`、`.claude/`、`*.key`、`node_modules/`）、本機 Git repo。建 GitHub repo、登錄 Obsidian 筆記都要使用者同意，且需先完成對應的懶人包（02、03）。
3. 安裝工作流程技能（見下節）。不要套用 OpenCode 的 `07-workflow-skills`。

## 安裝三個工作流程技能（只裝缺的）

本 repo 的 `workflow-skills/` 資料夾附了三個技能。先檢查 `~/.claude/skills/` 是否已有同名資料夾；**已有的不要覆蓋**，有差異時請使用者決定。

| 技能 | 用途 | repo 內路徑 |
|---|---|---|
| `project-init-sync` | 新專案初始化 | `workflow-skills/project-init-sync/SKILL.md` |
| `startup-sync` | 開工接續 | `workflow-skills/startup-sync/SKILL.md` |
| `shutdown-sync` | 收工同步 | `workflow-skills/shutdown-sync/SKILL.md` |

- 已有本機資料夾：複製對應資料夾到 `~/.claude/skills/<技能名稱>/`。
- 只拿到網址：從 `https://raw.githubusercontent.com/tedchen0814/claude-code-starter-packs/main/workflow-skills/<技能名稱>/SKILL.md` 下載，存成 `~/.claude/skills/<技能名稱>/SKILL.md`。
- 這三個技能會常駐在每個對話的技能清單裡（只佔名稱與說明）；安裝前先說明要裝哪三個，等使用者同意。裝完提醒：要**完全關閉 Claude Code 再重開**才會載入。

## 開工／收工
- 開工（`startup-sync`）：讀 `CLAUDE.md` → 讀 `工作筆記.md` → `git status`（只檢查，不 pull）→ 回報上次做到哪與下一步。
- 收工（`shutdown-sync`）：檢查敏感資料 → 更新 `工作筆記.md` → 明確檔案 `git commit` → 有 origin 時 push（push 前先確認）。
- 有安裝同步工具（chezmoi）才需要同步設定；沒有就略過，不要自行安裝。

回報：已建立的檔案清單、（若有）GitHub repo URL 與 Obsidian 筆記路徑。