# Workshop Setup Instructions

> **這份文件是給 Cursor AI 看的。** 學員把這份文件拖進 Cursor 的 Composer / Agent，跟它說「請依照這份文件幫我設定環境」，它會帶學員一步步做完。
>
> **前提（學員已完成）：** git clone 已經做好，學員目前就在 `claude-code-workshop-jr-student/` 資料夾裡。Cursor 開的是這個資料夾。
>
> 學員的角色：**看、確認、按 Allow、偶爾在瀏覽器按授權**。

---

## 給 Cursor AI 的核心規則

你正在幫一位**非 CS 背景的 Tech Management 碩士生**設定 AI CLI 環境。請嚴格遵守以下原則：

1. **用繁體中文跟學員溝通**，技術名詞英文。不要假設他懂 `npm`、`brew`、`export` 是什麼——每個動詞都先用一句話解釋。
2. **每個 Step 開始前，先告訴學員「我接下來要做什麼、為什麼」**，然後等學員說 OK 再開始。
3. **執行任何 sudo / 修改 shell config / 開外部網站前，務必先問學員確認。**
4. **跑完每個指令，看 output 確認成功，再進下一步。** 失敗就停下來解釋，不要硬往下。
5. **如果某步驟學員系統已經做好了，跳過。** 例如 `node -v` 顯示 v18+，就不用再裝 Node.js。
6. **不確定的時候，問學員，不要猜。**

---

## Step 1 · 詢問學員要裝哪些工具

**告訴學員，並等他回答：**
> 「今天 workshop 會用到 AI CLI 工具——在 terminal 裡跑的 AI 助手。有兩個選擇：
>
> - **Claude Code**（Anthropic 出的）→ 需要 Claude Pro/Max 訂閱帳號
> - **Codex CLI**（OpenAI 出的）→ 需要 ChatGPT Plus/Pro 訂閱帳號
>
> 你有哪個訂閱？想裝哪個（或兩個都裝）？」

根據學員回答，決定後續要走哪幾條路：
- 只裝 Claude Code → 做 Step 3A + Step 4A
- 只裝 Codex CLI → 做 Step 3B + Step 4B
- 兩個都裝 → 做 Step 3A + 3B + 4A + 4B

---

## Step 2 · 偵測系統與現有環境

**告訴學員：**
> 「我先檢查你的系統現有狀態——看你已經有什麼、還缺什麼。這個步驟不會改任何東西。」

**執行以下檢查並把結果報給學員：**

```bash
# macOS / Linux
echo "===== OS ====="
uname -s

echo "===== Node.js ====="
node -v 2>/dev/null || echo "❌ NOT INSTALLED"

echo "===== npm ====="
npm -v 2>/dev/null || echo "❌ NOT INSTALLED"

echo "===== Claude Code ====="
claude --version 2>/dev/null || echo "❌ NOT INSTALLED"

echo "===== Codex CLI ====="
codex --version 2>/dev/null || echo "❌ NOT INSTALLED"

echo "===== Homebrew (macOS) ====="
which brew 2>/dev/null || echo "❌ NOT INSTALLED (沒裝沒關係)"
```

Windows PowerShell：
```powershell
Write-Host "===== OS ====="; (Get-CimInstance Win32_OperatingSystem).Caption
Write-Host "===== Node.js ====="; try { node -v } catch { "❌ NOT INSTALLED" }
Write-Host "===== npm ====="; try { npm -v } catch { "❌ NOT INSTALLED" }
Write-Host "===== Claude Code ====="; try { claude --version } catch { "❌ NOT INSTALLED" }
Write-Host "===== Codex CLI ====="; try { codex --version } catch { "❌ NOT INSTALLED" }
```

**根據結果，告訴學員接下來要做哪幾個 Step**（已裝好的跳過）。

---

## Step 2.5 · 安裝 Node.js v18+（若尚未安裝）

> **跳過條件：** `node -v` 是 v18 或以上，直接跳到 Step 3。

**告訴學員：**
> 「Node.js 是這兩個工具共用的基底——就像引擎裡的汽油，沒它跑不起來。我幫你裝。」

### 路徑 A：macOS + 已有 Homebrew
```bash
brew install node
```

### 路徑 B：macOS 沒有 Homebrew

**告訴學員：**
> 「你的 Mac 沒有 Homebrew（常用的套件管理工具）。我們直接去官網下載安裝檔。等下會打開瀏覽器，你下載 LTS（長期支援版），雙擊跑完安裝程式後告訴我。」

打開 https://nodejs.org/，等學員回報安裝完成。

### 路徑 C：Windows

打開 https://nodejs.org/，讓學員下載 .msi 安裝檔，**完成後請重新打開 terminal**。

### 驗證（共通）
```bash
node -v   # 應該看到 v18.x.x 或更新
npm -v    # 應該看到版本號
```

---

## Step 3A · 安裝 Claude Code（有 Claude Pro/Max 的學員）

> **跳過條件：** `claude --version` 有結果，直接跳到 Step 4A。

**告訴學員：**
> 「Claude Code 是 Anthropic 出的 AI CLI——它能讀檔、寫檔、跑命令，跟你的檔案系統直接互動，不只是聊天。」

```bash
npm install -g @anthropic-ai/claude-code
```

### 常見錯誤與處理

| 錯誤 | 處理方式 |
|------|---------|
| `EACCES: permission denied`（macOS） | 跑 `sudo npm install -g @anthropic-ai/claude-code`，會請學員輸入 Mac 登入密碼 |
| `EACCES`（不想用 sudo） | `mkdir -p ~/.npm-global && npm config set prefix '~/.npm-global'`，再加 `export PATH=~/.npm-global/bin:$PATH` 到 `~/.zshrc`，重 source 後重跑 |
| `npm: command not found` | Step 2.5 沒做好，回去補 |
| Windows「拒絕存取」 | 用「以系統管理員身分執行 PowerShell」重開 |

### 驗證
```bash
claude --version
```

---

## Step 3B · 安裝 Codex CLI（有 ChatGPT Plus/Pro 的學員）

> **跳過條件：** `codex --version` 有結果，直接跳到 Step 4B。

**告訴學員：**
> 「Codex CLI 是 OpenAI 出的 AI CLI——跟 Claude Code 定位相同，都是在 terminal 裡幫你讀檔、寫檔、執行任務的 AI 助手。」

```bash
npm install -g @openai/codex
```

### 常見錯誤與處理（同 Step 3A）

| 錯誤 | 處理方式 |
|------|---------|
| `EACCES: permission denied`（macOS） | 跑 `sudo npm install -g @openai/codex` |
| `EACCES`（不想用 sudo） | 同 Step 3A 的 npm-global 方案 |
| `npm: command not found` | 回去 Step 2.5 |
| Windows「拒絕存取」 | 以系統管理員身分執行 PowerShell |

### 驗證
```bash
codex --version
```

---

## Step 4A · 登入 Claude Code（Claude Pro/Max 訂閱）

**告訴學員：**
> 「現在把 Claude Code 跟你的 Anthropic 帳號（就是平常用 Claude.ai 那個）連起來。」

```bash
claude
```

第一次跑 `claude` 會跳出登入流程：
- 顯示一段 URL
- 學員用瀏覽器打開，用 Claude.ai 帳號登入授權
- 回 terminal，看到歡迎訊息 = 成功

**告訴學員：** 「看到 Claude Code 的對話介面就是成功。輸入 `/exit` 先退出，繼續下一步。」

---

## Step 4B · 登入 Codex CLI（ChatGPT Plus/Pro 訂閱）

**告訴學員：**
> 「現在把 Codex 跟你的 OpenAI / ChatGPT 帳號連起來。」

```bash
codex auth
```

- 終端機會顯示一段 URL 或 one-time code，並跳出瀏覽器
- 學員用 ChatGPT 帳號登入授權
- 回 terminal，看到登入成功訊息 = 完成

### 驗證
```bash
codex --version   # 應看到版本號
```

> 若 `codex auth` 指令不存在（舊版本），改設 `OPENAI_API_KEY`：
> - 到 https://platform.openai.com/api-keys 建立 key
> - macOS：`echo 'export OPENAI_API_KEY="<key>"' >> ~/.zshrc && source ~/.zshrc`
> - Windows：`[Environment]::SetEnvironmentVariable("OPENAI_API_KEY","<key>","User")`

---

## Step 5 · 環境完成 → 交棒給 Claude Code

**告訴學員：**
> 「✅ 環境設定完成。
>
> **已安裝：**
> - Node.js：[印當前版本]
> - [已裝的工具列表，例如：Claude Code v1.x.x ✅ / Codex CLI vx.x.x ✅]
> - 登入：已完成 ✅
>
> 現在請**離開 Cursor**，回到 terminal。你已經在 `claude-code-workshop-jr-student/` 資料夾裡了，啟動 Claude Code：
>
> ```bash
> claude
> ```
>
> （用 Codex 的學員也可以跑 `codex`）
>
> 啟動後還有兩個小設定（Step 6、Step 7），這次換 **Claude Code** 帶你做。」

---

## Step 6 · 狀態列面板（在 Claude Code 裡做）

> **接力給 Claude Code。** 學員啟動 `claude` 後，把 `panel-setup.md` 餵給 Claude Code（拖進來，或說「請依照 panel-setup.md 幫我裝狀態列面板」）。
>
> 完成標準：Claude Code 畫面最底下出現狀態列。

---

## Step 7 · GitHub（在 Claude Code 裡做）

> **接力給 Claude Code。** 把 `github-setup.md` 餵給 Claude Code（拖進來，或說「請依照 github-setup.md 幫我設定 GitHub」）。
>
> 完成標準：學員的 GitHub 上出現一個新 repo，裡面有檔案。

---

> ✅ **全部完成 = Step 1–7 都做完。**

---

## 失敗模式速查表（給 Cursor AI 與講師）

| 症狀 | 可能原因 | 解法 |
|------|---------|------|
| `npm: command not found` | Node.js 沒裝好 / PATH 沒重 load | 重開 terminal；若還是不行回 Step 2.5 |
| `EACCES` 安裝工具 | 全域 npm 權限不足 | `sudo` 或改 `~/.npm-global` |
| `claude: command not found`（裝完之後） | npm 全域路徑不在 PATH | `which npm`，找出 prefix，把 `<prefix>/bin` 加到 `~/.zshrc` PATH |
| `codex: command not found`（裝完之後） | 同上 | 同上 |
| `claude` 登入頁沒出現 | 網路問題 / 已登入過 | 跑 `claude --dangerously-skip-permissions` 直接進介面 |
| `codex auth` 沒有這個指令 | 舊版 codex | 改設 `OPENAI_API_KEY`（見 Step 4B 備案） |
| Cursor AI 自己卡住或亂答 | 網路 / Cursor 服務問題 | 講師接手，改用手動 setup |

---

## 講師救援包

1. **Claude Code 卡登入：** 備一把 API key 紙片，學員用 `echo 'export ANTHROPIC_API_KEY="..."' >> ~/.zshrc && source ~/.zshrc` 設定後直接進入
2. **Codex 卡登入：** 備一把 OpenAI API key，學員用 `OPENAI_API_KEY` 環境變數頂著
3. **跳過策略：** 學員卡超過 8 分鐘，先用備用 key 讓他進去，workshop 後再補——不要 30 個人陪 1 個人等
