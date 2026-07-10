# 課前準備 + 新手操作小抄

> 給**完全沒寫過程式、對編輯器/終端機陌生**的同學。上課 90% 的卡關都來自下面這幾件「操作」小事——先看過，當天會順很多。
> 看不懂沒關係，**先做能做的，卡住就標記是哪一步**，當天直接問。

---

## 一、課前 30 分鐘 checklist

- [ ] 下載並安裝 **Antigravity**（Google 的 AI IDE）→ <https://antigravity.google/>（選對應你系統的版本）
- [ ] 用 **Google 帳號**登入 Antigravity
- [ ] 註冊一個 **GitHub** 帳號 → <https://github.com/signup>
- [ ] **把 workshop 素材 clone 下來**（不會做沒關係，當天開場會一起做）：用 Antigravity 開它的 AI，貼上並執行
  ```
  cd ~/Desktop && git clone https://github.com/museReed/claude-code-workshop-jr-student.git
  ```
  然後用 Antigravity **開這個資料夾**——之後餵給 AI 的 `to_IDE_AI_Agent_setup.md`、`to_CLI_AI_Agent_setup.md` 都預設你人已經在裡面。
- [ ] （選讀）先看過今天會用到的檔案（clone 後都在 `給AI_Agent的環境安裝文件/` 裡）：
  - [`to_IDE_AI_Agent_setup.md`](../給AI_Agent的環境安裝文件/to_IDE_AI_Agent_setup.md)（IDE 階段：裝環境）
  - [`to_CLI_AI_Agent_setup.md`](../給AI_Agent的環境安裝文件/to_CLI_AI_Agent_setup.md)（CLI 階段：GitHub + 面板）
  - [`github-setup.md`](../給AI_Agent的環境安裝文件/github-setup.md)（GitHub 設定細節）
  - [`panel-setup.md`](../給AI_Agent的環境安裝文件/ClaudeCodeSkill/panel-setup.md)（狀態列面板）
- [ ] 確認可以**分享螢幕**（Google Meet / Zoom 都先測一次）

> 裝不起來？不用焦慮，當天有時間救援。能先裝好就先裝。

---

## 二、操作小抄（最常卡的 8 件事）

### 1. Antigravity 登入：用 Google 帳號
打開 Antigravity 後用 **Google 帳號**登入即可。若看到任何要付費 / 升級（Upgrade / Pro）的方案，**先不要點**，今天用預設的免費額度就夠。

### 2. 切換視窗（最常迷路）
- Mac：**四/五指往上滑**看所有視窗，或 **Cmd + Tab** 切 App，或點下方 **Dock** 的圖示。
- 分不清「Antigravity 桌面 App」和「瀏覽器裡的網頁」→ 桌面 App 是一個獨立程式，網頁是在 Chrome/Safari 裡。**我們要用的是桌面 App**。

### 3. 分享螢幕：分享「整個螢幕」，不要分享單一視窗
- 選 **整個螢幕（Entire Screen）**，不要選某一個視窗。
- **不要中途按「停止共享」**——你切視窗時畫面會自己跟著換，不用重按。

### 4. 🔑 複製貼上指令：確認前後「沒有多餘空格或字」
這是**最常見的失敗**。從網頁複製指令貼到輸入框時，常會黏到一個空格或半個字，導致它「認不出檔案/指令」。
- 貼上後**檢查開頭和結尾乾不乾淨**；不確定就刪掉重貼。
- 失敗時 80% 是這個原因。

### 5. Claude Code 是「終端機介面」：以鍵盤為主，新版也能用滑鼠
進到 Claude Code（黑底文字介面）後，主要用鍵盤操作：
- 選項：用 **方向鍵 ↑↓** 移動，**Enter** 確認。
- 取消正在做的事：按 **Esc**（連按兩次可回到上一步）。
- 換行不送出：**Shift + Enter**。

💡 **新版本（目前的 `v2.1.200` 已支援）也能用滑鼠**：可以用滑鼠**點一下輸入框裡的位置移動文字游標**，也可以**反白選取文字後直接複製**。若你的滑鼠點不動，多半是版本較舊（舊版是純鍵盤），升級即可：`npm install -g @anthropic-ai/claude-code@latest`。

### 6. `@` 引用檔案（口語「小老鼠」）
要叫 AI 讀某個檔案時打 `@檔名`：
- `@` 後面**緊接檔名**，前後**不要黏到其他字或空格**。
- 打幾個字它會**自動跳出補全清單**，用方向鍵選、Enter 帶入。

### 7. Slash 指令 `/...` 要貼進「Claude Code 的輸入框」
像 `/plugin install ...`、`/claude-hud:setup` 這種**斜線開頭**的是 Claude Code 的指令：
- 貼到 **Claude Code 的輸入框**（不是 Mac 的終端機、也不是 Antigravity 的聊天框）。
- 一行一行貼、一行一行按 Enter。

### 8. 💻 Windows 專屬：裝完工具「關掉 IDE 重開」
用 winget / PowerShell 裝完 Node、`gh`、`claude` 這些工具後，**PATH 不會馬上生效**。把整個 **Antigravity 關掉、重新打開**，新開的 terminal 才認得剛裝好的指令。裝完卻說「找不到指令 / command not found」，先重開 IDE 再說，不用急著重裝。

---

## 三、卡關自救三步
1. **先問 Claude/Antigravity 本人**：直接用白話打「我現在卡在 X，畫面顯示 Y，我該怎麼辦？」——它很有耐心，笨問題都能問。
2. **看它回你的訊息**：它常常一步一步告訴你下一步要做什麼，**逐行讀**。
3. **真的不行**：記下「卡在哪一步、畫面顯示什麼」，當天直接問講師。

> 今天的目標不是把你變工程師，是讓你體會「**指揮 AI 幫你做事**」。操作小事卡住很正常，過了就會了。
