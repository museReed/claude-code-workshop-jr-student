# GitHub 設定指南（註冊 → 建 repo → push）

> **這份文件是給 AI Agent（Claude Code 或 Codex）看的。** 學員把這份文件餵給他的 AI CLI（拖進來，或說「請依照 github-setup.md 幫我設定 GitHub」），它會帶學員把帳號接好、建立一個個人 repo、並把成果 push 上去。
>
> **前提（學員已完成）：** git 已可用（Phase B 的 clone 成功就代表 git 正常），學員目前在 `claude-code-workshop-jr-student/` 資料夾裡。
>
> 目標：**人工介入越少越好**。學員只需要做兩件事：(1) 在瀏覽器註冊 GitHub 帳號、(2) 在 `gh auth login` 時於瀏覽器按授權。其餘全部由 AI Agent 自主執行。

---

## 給 AI Agent 的核心規則

你正在幫一位**非工程背景的初學者**設定 GitHub。請嚴格遵守：

1. **用繁體中文溝通**，技術名詞英文；每個動作先一句話說明「我要做什麼、為什麼」。
2. **執行任何指令前先說明，跑完看 output 確認成功再進下一步**，失敗就停下來解釋。
3. **只有兩個動作需要學員親自做**：瀏覽器註冊、`gh auth login` 的瀏覽器授權。其餘你自己跑。
4. **不要把任何 token / 密碼寫進檔案**。`gh auth login` 會安全地保管憑證。
5. 不確定就問學員，不要猜。

---

## Step 1 · 註冊 GitHub 帳號（學員手動，瀏覽器）

**告訴學員：**
> 「先註冊一個免費 GitHub 帳號。我幫你開註冊頁，你填 email、設密碼、選一個 username（之後會出現在你的作品網址裡，建議用好記的英文名）。驗證完 email 後回來告訴我，並把你的 **username** 給我。」

開啟 <https://github.com/signup>，等學員回報「註冊好了」+ 提供 username。

> 已經有 GitHub 帳號的學員：直接跳到 Step 2。

---

## Step 2 · 安裝 GitHub CLI（`gh`）

> **跳過條件：** 先跑 `gh --version`，有版本號就跳到 Step 3。

**告訴學員：** 「我裝一個叫 `gh` 的小工具，它讓我可以直接幫你操作 GitHub。**我直接幫你裝好，你不用上任何網站點。**」

> ⚠️ **不要**把學員導去 `cli.github.com` 手動下載。依系統選一條路徑，用指令裝好。

```bash
# macOS（有 Homebrew）
brew install gh

# macOS（沒有 Homebrew）→ 你直接抓官方 binary（免管理員、不碰網站）
GH_VER=$(curl -fsSL https://api.github.com/repos/cli/cli/releases/latest \
  | grep -m1 '"tag_name"' | sed -E 's/.*"v?([^"]+)".*/\1/')
ARCH=$([ "$(uname -m)" = "arm64" ] && echo arm64 || echo amd64)
curl -fL -o /tmp/gh.zip \
  "https://github.com/cli/cli/releases/download/v${GH_VER}/gh_${GH_VER}_macOS_${ARCH}.zip"
unzip -oq /tmp/gh.zip -d /tmp
# 放進 npm 全域 bin：claude/codex 就裝在這，一定在 PATH、免管理員權限
DEST="$(npm config get prefix)/bin"
mkdir -p "$DEST" && cp "/tmp/gh_${GH_VER}_macOS_${ARCH}/bin/gh" "$DEST/gh"

# Windows（系統內建 winget，不碰網站）
winget install --id GitHub.cli -e --source winget
```

驗證：
```bash
gh --version
```

---

## Step 3 · 登入 GitHub（`gh auth login`，學員瀏覽器按授權）

**告訴學員：** 「現在把這台電腦跟你的 GitHub 帳號連起來。我跑登入指令，會跳出一個一次性代碼，你在瀏覽器貼上、按授權就好。」

執行（用這組非互動參數，盡量減少提問）：
```bash
gh auth login --hostname github.com --git-protocol https --web
```

- 終端機會顯示一段 **one-time code**，並開啟瀏覽器。
- 請學員把 code 貼到瀏覽器、按 **Authorize**。
- 回到終端機看到 `✓ Logged in as <username>` = 成功。

驗證：
```bash
gh auth status
```

設定 git 身分（若尚未設定，用 GitHub 帳號資訊）：
```bash
git config --global user.name  "$(gh api user --jq .login)"
git config --global user.email "$(gh api user --jq '.id')+$(gh api user --jq .login)@users.noreply.github.com"
```

---

## Step 4 · 建立個人 repo 並 push（AI Agent 自主執行）

**告訴學員：** 「接下來我自己來：**在你桌面另外開一個「你自己的」成果 repo**（跟老師的素材資料夾分開），以後你的作品都放那。你看著就好。」

> ⚠️ **職責邊界（避免搞混）：** 現在所在的 `claude-code-workshop-jr-student/` 是**老師的 workshop 素材 repo（museReed 的），不要 push 到這裡**。下面的指令會在 `~/Desktop/my-claude-workshop/` 另建一個**學員自己帳號下**的成果 repo（`claude-code-workshop`），兩者分開。

```bash
# 1. 建立個人成果資料夾（與 workshop 素材分開）
mkdir -p ~/Desktop/my-claude-workshop
cd ~/Desktop/my-claude-workshop

# 2. 初始化 git
git init -b main

# 3. 建立起始 README
printf '# 我的 Claude Code Workshop 成果\n\n今天學到的東西都在這裡。\n' > README.md

# 4. 第一次 commit
git add README.md
git commit -m "First commit from Claude Code Workshop"

# 5. 建立 GitHub repo 並 push
gh repo create "claude-code-workshop" --private --source=. --remote=origin --push
```

> 若 `gh repo create` 說 repo 已存在：改用 `git remote add origin https://github.com/<username>/claude-code-workshop.git && git push -u origin main`。

**告訴學員：** 「好了，你有自己的 GitHub repo 了！來看看：」
```bash
gh repo view --web
```

---

## 驗證

```bash
gh repo view --web   # 瀏覽器打開你的新 repo
```
在 GitHub 頁面看到 `README.md` = **成功**。

---

## 之後怎麼用（一句話）

以後每次想備份成果，在 `~/Desktop/my-claude-workshop/` 裡跑這三行（或直接叫你的 AI CLI（Claude Code / Codex）「幫我 push 上 GitHub」）：
```bash
git add -A && git commit -m "update" && git push
```

---

## 常見錯誤

| 症狀 | 處理 |
|------|------|
| `gh: command not found` | Step 2 沒裝好 / 沒重開 terminal；重做 Step 2 |
| `gh auth login` 卡住 | 確認瀏覽器有貼 one-time code 並按 Authorize；可重跑指令 |
| `repository already exists` | 改跑 `git remote add origin ... && git push -u origin main` |
| push 被拒（non-fast-forward） | 先 `git pull --rebase origin main` 再 `git push` |
| 學員沒有 Homebrew 也裝不了 gh | 改用官方安裝頁 <https://cli.github.com/>，或先跳過、workshop 後補做 |
