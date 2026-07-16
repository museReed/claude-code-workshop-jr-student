# Workshop Setup Instructions

> **這份文件是給 IDE 裡的 AI Agent 看的（例如 Antigravity）。** 學員把這份文件交給 IDE 的 AI Agent，跟它說「請依照這份文件幫我設定環境」，它會帶學員裝好環境（Node + Claude Code / Codex CLI）並提醒註冊 GitHub。之後的 **GitHub 設定與狀態列面板**，見同資料夾的 `to_CLI_AI_Agent_setup.md`（給 terminal 裡的 CLI Agent 用）。
>
> **前提（學員已完成）：** git clone 已經做好，學員目前就在 `claude-code-workshop-jr-student/` 資料夾裡，AI Agent 開的是這個資料夾。
>
> 學員的角色：**看、確認、在瀏覽器按授權**。

---

## 給 AI Agent 的核心規則

你正在幫一位**非工程背景的初學者**設定 AI CLI 環境。請嚴格遵守以下原則：

1. **用繁體中文跟學員溝通**，技術名詞英文。不要假設他懂 `npm`、`brew`、`export` 是什麼——每個動詞都先用一句話解釋。
2. **每個 Step 開始前，先告訴學員「我接下來要做什麼、為什麼」**，然後等學員說 OK 再開始。
3. **執行任何 sudo / 修改 shell config / 開外部網站前，務必先問學員確認。**
   **⚠️ sudo 一定會在 terminal 跳出「Password:」等學員輸入——你（AI Agent）代打不了。** 跑任何 `sudo` 指令前，先明確告訴學員：「接下來 terminal 會要你輸入**這台電腦的開機密碼**（打字時不會顯示，正常的），請你自己打、按 Enter。」跑下去後若畫面卡在 `Password:` 不動，就是在等學員輸入，別以為當機。
4. **跑完每個指令，看 output 確認成功，再進下一步。** 失敗就停下來解釋，不要硬往下。
5. **如果某步驟學員系統已經做好了，跳過。** 例如 `node -v` 顯示 v18+，就不用再裝 Node.js。
6. **不確定的時候，問學員，不要猜。**
7. **⚠️ Windows 學員特別注意：用 winget / PowerShell 裝完任何工具（Node、`gh`、`claude`…）後，PATH 不會馬上生效。** 請學員**把整個 IDE（Antigravity）關掉、重新開啟**，新開的 terminal 才吃得到剛裝好的指令。裝完卻 `command not found`，九成是這個——別急著重裝，先重開 IDE。
8. **⚠️ Windows PowerShell Execution Policy：** Windows 預設的 PowerShell Execution Policy 是 `Restricted`，會阻擋所有 `.ps1` 腳本執行。透過 npm 全域安裝的 CLI 工具（`claude`、`codex`、`gh`）都會產生 `.ps1` 包裝腳本，PowerShell 會優先嘗試執行它們，導致 `cannot be loaded because running scripts is disabled` 錯誤。**處理方式（二選一）：**
   - **快速繞過（推薦）：** 改用 `.cmd` 後綴呼叫，例如 `claude.cmd`、`codex.cmd`。`.cmd` 檔不受 Execution Policy 限制。
   - **根本修復：** 執行 `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser`（只影響當前使用者，不需要管理員權限），之後就能直接用 `claude`、`codex`。
   - **偵測方式：** 在 Step 2 偵測環境時，先跑 `Get-ExecutionPolicy`，若回傳 `Restricted` 就主動提醒學員並協助修正。

---

## Step 1 · 詢問學員要裝哪些 AI 工具

**告訴學員，並等他回答：**
> 「今天 workshop 會用到 AI CLI 工具——在 terminal 裡跑的 AI 助手。有兩個選擇，**兩個都需要付費訂閱帳號**（約 US$20/月起）：
>
> - **Claude Code**（Anthropic 出的）→ 需要 **Claude Pro 或 Max** 訂閱
> - **Codex CLI**（OpenAI 出的）→ 需要 **ChatGPT Plus 或 Pro** 訂閱
>
> ⚠️ **課程不提供共用金鑰（API key）**，需要用你自己的訂閱帳號。你有哪個訂閱？想裝哪個（或兩個都裝）？」

根據學員回答，決定後續要走哪幾條路：
- 只裝 Claude Code → 做 Step 3A + Step 4A
- 只裝 Codex CLI → 做 Step 3B + Step 4B
- 兩個都裝 → 做 Step 3A + 3B + 4A + 4B

### 學員說「我兩個訂閱都沒有」

**告訴學員：**
> 「今天用的工具需要付費訂閱，課程不提供共用金鑰。**本課程主要示範 Claude Code，建議你申請它**（Claude Pro，約 US$20/月）：
>
> - 打開 <https://claude.ai>，登入後點 **Upgrade to Pro**（Pro 就夠上課，Max 是重度使用者才需要）。
>
> 申請 + 付款完成後回來告訴我，我們接著裝環境。真的不方便付費也沒關係——今天先看講師示範、之後自己補做。」

> **給講師：** 學員當場不想付費也沒關係——讓他看鄰座或講師的螢幕，跟著理解流程，環境當天先跳過。**不要用 API key 幫他頂替。**

---

## Step 2 · 偵測系統與現有環境

**告訴學員：**
> 「我先檢查你的系統現有狀態——看你已經有什麼、還缺什麼。這個步驟不會改任何東西。」

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

echo "===== GitHub CLI ====="
gh --version 2>/dev/null | head -1 || echo "❌ NOT INSTALLED"

echo "===== Homebrew (macOS) ====="
which brew 2>/dev/null || echo "❌ NOT INSTALLED (沒裝沒關係)"
```

Windows PowerShell：
```powershell
# 先偵測 Execution Policy
Write-Host "===== Execution Policy ====="
$ep = Get-ExecutionPolicy
Write-Host $ep
if ($ep -eq 'Restricted') {
  Write-Host "⚠️ Execution Policy 為 Restricted，npm 安裝的 CLI 工具 (.ps1) 會被阻擋"
  Write-Host "   → 建議執行: Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser"
  Write-Host "   → 或改用 .cmd 後綴呼叫（例如 claude.cmd）"
}

Write-Host "===== OS ====="; (Get-CimInstance Win32_OperatingSystem).Caption
Write-Host "===== Node.js ====="; try { node -v } catch { "❌ NOT INSTALLED" }
Write-Host "===== npm ====="; try { npm -v } catch { "❌ NOT INSTALLED" }
Write-Host "===== git ====="; try { git --version } catch { "❌ NOT INSTALLED（Step 4 建 repo 時需要，github-setup.md 會處理）" }

# 用 .cmd 後綴偵測，避免被 Execution Policy 阻擋
Write-Host "===== Claude Code ====="; try { claude.cmd --version } catch { "❌ NOT INSTALLED" }
Write-Host "===== Codex CLI ====="; try { codex.cmd --version } catch { "❌ NOT INSTALLED" }
Write-Host "===== GitHub CLI ====="; try { gh --version } catch { "❌ NOT INSTALLED" }
```

**根據結果，告訴學員接下來要做哪幾個 Step**（已裝好的跳過）。

---

## Step 2.5 · 安裝 Node.js v18+（若尚未安裝，**由你直接下載安裝，不要叫學員上網站**）

> **跳過條件：** `node -v` 是 v18 或以上，直接跳到 Step 3。

**告訴學員：**
> 「Node.js 是這兩個工具共用的基底——就像引擎裡的汽油，沒它跑不起來。**我直接幫你下載安裝，你不用上任何網站點。**」

> ⚠️ 依學員系統選一條路徑，**用指令把 Node 裝好**。**不要**把學員導去 nodejs.org 手動下載。

### 路徑 A：macOS + 已有 Homebrew
```bash
brew install node
```

### 路徑 B：macOS 沒有 Homebrew → 你直接抓官方安裝包裝好
安裝要用 `sudo`（第 3 步）。**依核心規則 3：先告訴學員「等一下 terminal 會跳出 Password:，請你自己輸入開機密碼、按 Enter」，再跑指令。** 卡在 `Password:` 是正常的，那是在等學員打字。
```bash
# 1. 取得目前 LTS 版本號（純 shell，不需額外工具）
LTS=$(curl -fsSL https://nodejs.org/dist/index.json \
  | tr '}' '\n' | grep -m1 '"lts":"[A-Za-z]' \
  | sed -E 's/.*"version":"([^"]+)".*/\1/')
echo "要安裝的 Node 版本：$LTS"

# 2. 下載官方「通用」安裝包（Intel / Apple Silicon 皆適用）
curl -fL -o /tmp/node-lts.pkg "https://nodejs.org/dist/$LTS/node-$LTS.pkg"

# 3. 安裝
sudo installer -pkg /tmp/node-lts.pkg -target /
```

### 路徑 C：Windows → 用系統內建的 winget 直接裝
```powershell
winget install --id OpenJS.NodeJS.LTS -e --source winget
```
裝完提醒學員照核心規則 7 **關掉 IDE 重開**，PATH 才生效。

### 驗證（共通）
```bash
node -v   # 應該看到 v18.x.x 或更新
npm -v
```

---

## Step 3A · 安裝 Claude Code（有 Claude Pro/Max 的學員）

> **跳過條件：** `claude --version` 有結果，直接跳到 Step 4A。

**告訴學員：**
> 「Claude Code 是 Anthropic 出的 AI CLI——它能讀檔、寫檔、跑命令，跟你的檔案系統直接互動，不只是聊天。」

```bash
npm install -g @anthropic-ai/claude-code
```

### 常見錯誤

| 錯誤 | 處理方式 |
|------|---------|
| `EACCES: permission denied`（macOS） | 跑 `sudo npm install -g @anthropic-ai/claude-code`（會跳 `Password:`，**先請學員自己輸入開機密碼**，見核心規則 3） |
| `EACCES`（不想用 sudo） | `mkdir -p ~/.npm-global && npm config set prefix '~/.npm-global'`，再加 `export PATH=~/.npm-global/bin:$PATH` 到 `~/.zshrc`，重 source 後重跑 |
| `npm: command not found` | 回去 Step 2.5 |
| Windows「拒絕存取」 | 以系統管理員身分執行 PowerShell |

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

### 常見錯誤（同 Step 3A）

| 錯誤 | 處理方式 |
|------|---------|
| `EACCES`（macOS） | `sudo npm install -g @openai/codex` |
| `npm: command not found` | 回去 Step 2.5 |
| Windows「拒絕存取」 | 以系統管理員身分執行 PowerShell |

### 驗證
```bash
codex --version
```

---

## Step 4A · 登入 Claude Code（Claude Pro/Max 訂閱）

**告訴學員：**
> 「現在把 Claude Code 跟你的 Anthropic 帳號（就是平常用 Claude.ai 那個）連起來。我跑指令，瀏覽器會打開，你登入授權就好。」

```bash
claude
```

- 顯示 URL，學員用瀏覽器打開，用 Claude.ai 帳號登入授權
- 回 terminal，看到歡迎訊息 = 成功
- 輸入 `/exit` 退出，繼續下一步

---

## Step 4B · 登入 Codex CLI（ChatGPT Plus/Pro 訂閱）

**告訴學員：**
> 「現在把 Codex 跟你的 OpenAI / ChatGPT 帳號連起來。」

```bash
codex login
```

- 跑完會開瀏覽器，學員選 **Sign in with ChatGPT**、用 ChatGPT 帳號登入授權
- 回 terminal，看到登入成功訊息 = 完成
- 可用 `codex login status` 確認目前登入狀態

> 若 `codex login` 指令不存在，多半是舊版：升級後重試 → `npm install -g @openai/codex@latest`。
> （登入一律用 ChatGPT 訂閱帳號授權，**不要改用 API key**。）

---

## Step 5 · 提醒學員註冊 GitHub 帳號（`gh` 設定留到後面）

**主動告訴學員，並把連結貼出來（現在就先做，別等）：**
> 「等一下要把你的成果放上 GitHub 備份。**如果你還沒有 GitHub 帳號，現在先花兩分鐘註冊一個**（免費）：
>
> 👉 <https://github.com/signup>
>
> 填 email、設密碼、選一個 username（會出現在你的作品網址，建議用好記的英文名），驗證完 email 就好。已經有帳號的跳過這步。」

> **職責分工：** `gh` 安裝與登入、建立個人 repo、push，**留到學員啟動 AI CLI（Claude Code 或 Codex）後，由它讀 `github-setup.md` 完成**（那份本來就負責建 repo + push，放同一份最連貫）。這裡**不要**裝 `gh`、也不要跑 `gh auth login`，避免和 `github-setup.md` 重複。你在這個檔案的任務，到「AI CLI 裝好能登入 ＋ 已提醒學員註冊 GitHub」就結束了。

---

## Step 6 · 環境完成 → 交棒給 CLI 裡的 AI Agent

**告訴學員：**
> 「✅ 環境都裝好了！
>
> **已完成：**
> - Node.js [版本] ✅
> - [已裝的 AI 工具，例如：Claude Code v1.x.x / Codex CLI vx.x.x] ✅
> - 已提醒你註冊 GitHub 帳號 ✅
>
> 接下來的 **GitHub 設定 + 狀態列面板**，改由你 **terminal 裡的 AI CLI** 帶你做。現在請**離開 Antigravity**，回到 terminal，啟動你剛裝的 AI CLI：
>
> ```bash
> claude     # 裝 Claude Code 的
> # 或
> codex      # 裝 Codex 的
> ```
>
> 啟動後，把 **`agent-setup/to_CLI_AI_Agent_setup.md`** 這份文件餵給它（拖進來，或說「請依照 agent-setup/to_CLI_AI_Agent_setup.md 繼續設定」），它會帶你把 GitHub 接好、（Claude Code 的話）再裝狀態列面板。」

---

> ✅ **這份文件（IDE 階段）到這裡結束** = Step 1–6 都做完。GitHub 與面板接續見 `to_CLI_AI_Agent_setup.md`。

---

## 失敗模式速查表（給 AI Agent 與講師）

| 症狀 | 可能原因 | 解法 |
|------|---------|------|
| `npm: command not found` | Node.js 沒裝好 / PATH 沒重 load | 重開 terminal；若還是不行回 Step 2.5 |
| `EACCES` 安裝工具 | 全域 npm 權限不足 | `sudo` 或改 `~/.npm-global` |
| `claude: command not found`（裝完之後） | npm 全域路徑不在 PATH | `which npm`，找出 prefix，把 `<prefix>/bin` 加到 `~/.zshrc` PATH |
| `codex: command not found`（裝完之後） | 同上 | 同上 |
| `claude` / `codex` 報 `cannot be loaded because running scripts is disabled`（Windows） | PowerShell Execution Policy 為 `Restricted`，阻擋 `.ps1` 腳本 | 改用 `claude.cmd` / `codex.cmd` 呼叫；或執行 `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser` 後重試 |
| `claude` 登入頁沒出現 | 網路問題 | 跑 `claude --dangerously-skip-permissions` 直接進介面 |
| `codex login` 沒有這個指令 | 舊版 codex | 升級 → `npm install -g @openai/codex@latest`，再重試登入 |
| `git: command not found`（Windows） | Windows 不預裝 git；學員可能是下載 ZIP 而非 git clone 取得教材 | `winget install --id Git.Git -e --source winget`，裝完後加 PATH：`$env:PATH = "C:\Program Files\Git\cmd;" + $env:PATH` |
| AI Agent 自己卡住或亂答 | 網路 / Antigravity 服務問題 | 講師接手，改用手動 setup |
| GitHub 相關問題（`gh` 裝不起來、登入卡住） | — | 屬 Claude Code 階段，見 `github-setup.md` 的常見錯誤 |

---

## 講師救援包

1. **AI 工具卡登入：** 確認學員的訂閱帳號有效、瀏覽器有按授權，再重跑登入指令。**課程不提供 API key，不要用金鑰頂替。**
2. **GitHub 卡登入：** 確認學員有在瀏覽器按 Authorize；`gh auth status` 確認狀態
3. **跳過策略：** 學員卡超過 8 分鐘，先讓他看鄰座或講師 demo，workshop 後再補——不要 30 個人陪 1 個人等
