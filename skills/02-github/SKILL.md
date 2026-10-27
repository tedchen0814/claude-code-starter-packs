---
name: cc-github
description: Claude Code 連接 GitHub CLI。說「連接 GitHub」時載入。
---

# 連接 GitHub（Claude Code 版）

1. 檢查 `git --version && gh --version`。
   - 缺少時先說明用途，經使用者同意再安裝（Windows：`winget install --id Git.Git -e`、`winget install --id GitHub.cli -e`；macOS：`brew install git gh`），裝完開新的終端機再檢查。
   - 已登入就不要重複登入：先跑 `gh auth status`（若在 Claude Code 的環境裡顯示失敗，請使用者在自己的終端機確認，不要直接判定 token 無效）。
2. `gh auth login --web --git-protocol https`（請使用者自己在瀏覽器完成授權，不要把 token 貼進對話）。
3. `git config --global user.name "你的名字"` + `git config --global user.email "..."`（先問使用者要用的名稱與信箱，不要猜）。
4. 測試：先說明會在 GitHub 建立一個**私有**測試 repo，經使用者同意後才做：建立測試 repo → commit → `gh repo create --private` → push，成功後詢問保留或刪除。

回報：Git／GitHub CLI 版本、登入狀態、使用者資訊、測試結果。
