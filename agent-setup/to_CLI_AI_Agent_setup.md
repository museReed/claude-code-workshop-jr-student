# CLI 階段設定（GitHub + 狀態列面板）

> **這份文件是給 terminal 裡的 AI CLI Agent 看的（Claude Code 或 Codex）。** 學員啟動 `claude` 或 `codex` 後，把這份文件餵給它（拖進來，或說「請依照 to_CLI_AI_Agent_setup.md 繼續設定」），它會接手把 GitHub 設定好、（Claude Code 的話）再裝狀態列面板。
>
> **前提（前一階段已完成）：** 環境已裝好（Node + Claude Code / Codex 已能登入）、學員已註冊 GitHub 帳號、人就在 terminal 的 `claude-code-workshop-jr-student/` 資料夾裡並已啟動 AI CLI。前一階段見 `to_IDE_AI_Agent_setup.md`。

---

## 給 AI Agent 的核心規則

你正在幫一位**非工程背景的初學者**繼續設定。請嚴格遵守：

1. **用繁體中文跟學員溝通**，技術名詞英文；每個動作先一句話說明「我要做什麼、為什麼」。
2. **執行任何指令前先說明，跑完看 output 確認成功再進下一步**，失敗就停下來解釋。
3. **不確定就問學員，不要猜。**
4. **⚠️ Windows PowerShell Execution Policy：** 若在 PowerShell 中執行 `claude`、`codex`、`gh` 等指令時出現 `cannot be loaded because running scripts is disabled` 錯誤，代表 Execution Policy 為 `Restricted`。**處理方式：** 改用 `.cmd` 後綴呼叫（如 `claude.cmd`），或執行 `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser` 後重試。
5. **⚠️ Windows PATH 問題：** 在 Windows 上用 `winget` 裝完工具（`gh`、`git`）後，當前 session 的 PATH 不會更新。**免重開方案：** 手動加 PATH 後繼續（如 `$env:PATH = "C:\Program Files\GitHub CLI;C:\Program Files\Git\cmd;" + $env:PATH`），不需要每次都叫學員重開 terminal 和重啟 CLI Agent。詳見 `github-setup.md` 各步驟的 Windows 指引。

---

## Step 1 · GitHub 設定

> GitHub 設定的**細部指令**在 `agent-setup/github-setup.md`（相對 repo 根目錄，學員人就在根目錄）——你（CLI Agent）把它讀進來、依步驟完成：裝 `gh`、帶學員登入、建立學員自己的個人 repo 並 push。
>
> 註：`github-setup.md` 只是**給你參考的細部文件**，學員餵的入口是這份 `to_CLI_AI_Agent_setup.md`，不必請學員另外餵 `github-setup.md`。
>
> 完成標準：學員自己帳號下出現 `claude-code-workshop` repo。

---

## Step 2 · 狀態列面板（**只有 Claude Code 版**，帶學員照網頁操作）

> **Claude Code 學員：** 這步不是你自動裝，而是**帶著學員打開下面網頁、照著步驟做**：
> 👉 <https://musereed.github.io/jr_workshop_setup_env/#/item/status-panel>
> 完成標準：Claude Code 畫面最底下出現狀態列。
>
> ⚠️ **Codex 學員：** 面板是 Claude Code 專屬（claude-hud），Codex 版尚未提供，**跳過這步**——做完 Step 1 就算全部就緒。

---

> ✅ **全部完成** = Claude Code 學員做完 Step 1–2；Codex 學員做完 Step 1。環境就緒，可以開始 workshop 練習了！
