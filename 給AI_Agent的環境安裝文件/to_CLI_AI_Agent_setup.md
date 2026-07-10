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

---

## Step 1 · GitHub 設定

> 把同資料夾的 `github-setup.md` 讀進來（學員拖進來，或說「請依照 github-setup.md 幫我設定 GitHub」），依它完成：裝 `gh`、帶學員登入、建立學員自己的個人 repo 並 push。
>
> 完成標準：學員自己帳號下出現 `claude-code-workshop` repo。

---

## Step 2 · 狀態列面板（**只有 Claude Code 版**）

> **Claude Code 學員：** 讀 `ClaudeCodeSkill/panel-setup.md`（學員拖進來，或說「請依照 panel-setup.md 幫我裝狀態列面板」）。
> 完成標準：Claude Code 畫面最底下出現狀態列。
>
> ⚠️ **Codex 學員：** 面板是 Claude Code 專屬（claude-hud），Codex 版尚未提供，**跳過這步**——做完 Step 1 就算全部就緒。

---

> ✅ **全部完成** = Claude Code 學員做完 Step 1–2；Codex 學員做完 Step 1。環境就緒，可以開始 workshop 練習了！
