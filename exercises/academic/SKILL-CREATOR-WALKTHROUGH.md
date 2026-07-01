# 用 /skill-creator 從零建出一個 Skill — 完整 Walkthrough

**對應 Slide**: 30–31（Academic Pipeline 場景 → 用 skill-creator 把它變成可重複使用的 skill）
**目的**: 展示「我有一個工作流程，怎麼讓 Claude Code 記住它」的完整過程
**時間**: ~20 分鐘 live demo

---

## 前情提要：為什麼需要 Skill？

> 你剛剛手動跑完了 Academic Pipeline — 4 份素材變成 1 份 chapter draft。
> 但下次換一篇論文、換一個主題，你還要重新想 prompt、重新記順序嗎？
>
> Skill 就是把這個 pipeline **固化成 Claude 的肌肉記憶**。
> 裝了 skill 之後，你只要說「幫我跑 academic pipeline」，Claude 就會自動走完 10 步。

---

## Phase 1 · 啟動 skill-creator（1 分鐘）

在 Claude Code 裡輸入：

```
/skill-creator
```

skill-creator 會問你想做什麼。回答：

```
create academic-chapter-draft
```

它會自動幫你建好骨架：

```
academic-chapter-draft/
├── SKILL.md          ← 你要寫的主檔（AI 讀這個來知道怎麼做）
├── scripts/          ← 放腳本（這次不用）
├── references/       ← 放參考資料（我們會放 prompt templates）
└── assets/           ← 放模板檔案（這次不用）
```

**教學重點**：「skill-creator 幫你搭好架子，你只需要填內容。就像 `npm init` 幫你生 package.json 一樣。」

---

## Phase 2 · 回答 skill-creator 的引導問題（3 分鐘）

skill-creator 會問 4 個問題。以下是我們的回答：

### Q1: What should this skill enable Claude to do?

> Transform scattered academic materials (papers, messy citations, survey CSV, outline)
> into a polished dissertation chapter draft through a 10-step pipeline.
> AI handles execution (steps 2, 3, 5, 6, 8), the student handles every decision
> and quality gate (steps 1, 4, 7, 9, 10).

### Q2: When should this skill trigger?

> When user mentions "chapter draft", "dissertation writing", "literature review pipeline",
> "論文寫作", "學術寫作", or has a working directory with lit-review/, references/,
> survey-analysis/, and dissertation/ folders.

### Q3: What's the expected output format?

> 4 intermediate files (comparison_table.md, refs_clean.md, key_stats.md)
> and 1 final output (chapter2_draft.md) with [needs citation] markers.

### Q4: Should we set up test cases?

> Yes — we have real test materials in exercises/academic/ to validate against.

**教學重點**：「你不需要自己從零寫 SKILL.md。skill-creator 用你的回答幫你起草初稿。但你必須知道你要什麼 — AI 建 skill 的品質取決於你描述需求的品質。」

---

## Phase 3 · 審查 + 修改 SKILL.md（5 分鐘）

skill-creator 會根據你的回答產出 SKILL.md 初稿。打開它，檢查以下重點：

### 3a. Frontmatter（前 5 行）

```yaml
---
name: academic-chapter-draft
description: >
  Transform scattered academic materials into a polished chapter draft...
---
```

- `name` = skill 的 ID，之後觸發用
- `description` = Claude 判斷「要不要啟用這個 skill」的依據。**寫得太模糊 → skill 不會被觸發**

### 3b. Pipeline 步驟是否完整對應 Flowchart

打開 Slide 31 的 Interactive Flowchart，逐一比對：

| Flowchart 節點 | SKILL.md 對應 | 檢查 |
|----------------|---------------|------|
| 1. 👤 啟動 | Step 1 · 啟動 | 有沒有要求列出目錄結構？ |
| 2. 🤖 探索素材 | Step 2 · 探索素材全覽 | 有沒有角色分配？ |
| 3. 🤖 文獻比較 | Step 3 · 文獻比較表 | 有沒有 `[unclear]` 規則？ |
| 4. 👤 校對 | Step 4 · 校對表格 | 是不是 Human checkpoint？ |
| 5. 🤖 整理引用 | Step 5 · 整理引用格式 | 有沒有「不猜」的規則？ |
| 6. 🤖 挖數字 | Step 6 · 挖關鍵數字 | 有沒有要求標反例？ |
| 7. 👤 評估 | Step 7 · 評估數字相關性 | 是不是 Human checkpoint？ |
| 8. 🤖 撰寫草稿 | Step 8 · 撰寫章節草稿 | 有沒有 `[needs citation]`？ |
| 9. 👤 補引用 | Step 9 · 補上真實引用 | 是不是學生的事？ |
| 10. 👤 改寫 | Step 10 · 改寫成自己的字 | 有沒有強調「用自己的話」？ |
| ✅ Done | Done section | 最終產出描述是否完整？ |

### 3c. Failure Paths 是否對應

```
Step 4 → Step 3  表格有誤
Step 7 → Step 6  數字不相關
Step 8 → Step 3  文獻支撐不足
Step 10 → Step 8 段落不懂
```

### 3d. Common Mistakes

確認至少涵蓋：
1. 編造引用（#1 失敗模式）
2. 猜填 `[unclear]`
3. 跳過 human checkpoint
4. 模糊 topic sentence
5. 隱藏反例

**教學重點**：「SKILL.md 不是寫給人看的 README — 它是寫給 Claude 看的操作手冊。每一條規則都會影響 Claude 的行為。」

---

## Phase 4 · 抽出 Prompt Templates 到 references/（2 分鐘）

SKILL.md 裡不放完整 prompt（太長會撐爆 context）。改放在 `references/prompts.md`，SKILL.md 用指針引用：

```markdown
Read `references/prompts.md` § "Step 3 Prompt" for the exact prompt template.
```

`references/prompts.md` 裡放 5 個 AI 步驟的完整 prompt template：

```
## Step 2 Prompt
（探索素材的 prompt）

## Step 3 Prompt
（文獻比較表的 prompt）

## Step 5 Prompt
（整理引用的 prompt）

## Step 6 Prompt
（挖數字的 prompt）

## Step 8 Prompt
（撰寫草稿的 prompt）
```

**教學重點**：「這是 skill 的分層設計 — SKILL.md 是永遠載入的（~130 行），references/ 是需要時才讀的。你不想每次對話都佔掉太多 context window。」

---

## Phase 5 · 跑一次測試（5 分鐘）

把 skill 放到正確位置：

```
.claude/skills/academic-chapter-draft/
├── SKILL.md
└── references/prompts.md
```

然後開一個新的 Claude Code session，在 `exercises/academic/` 目錄下：

```bash
cd exercises/academic/
claude
```

測試 prompt：

```
我有 3 篇論文、一份混亂引用、一份問卷 CSV、一份大綱。幫我跑 academic pipeline。
```

**觀察 Claude 是否：**
1. ✅ 自動載入了 academic-chapter-draft skill
2. ✅ 先列出目錄結構（Step 2）
3. ✅ 產出 comparison_table.md 時用了 `[unclear]`（Step 3）
4. ✅ 在 Step 4 停下來等你確認（Human checkpoint）
5. ✅ 產出 key_stats.md 時標出反例（Step 6）
6. ✅ 草稿用了 `[needs citation]` 而非編造引用（Step 8）

**如果有問題**：回去改 SKILL.md → 重新測試。這就是 skill-creator 的 iterate loop。

---

## Phase 6 · 打包分發（1 分鐘）

Skill 放在 `.claude/skills/` 目錄下。學生 clone repo 後就自動有：

```bash
git clone https://github.com/museReed/claude-code-workshop-jr-student.git
cd claude-code-workshop-jr-student/exercises/academic/
claude   # ← skill 自動生效
```

不需要額外安裝步驟。

---

## 總結：Skill 建立的 6 個 Phase

```
Phase 1  啟動         /skill-creator create <name>
Phase 2  描述需求     回答 4 個引導問題
Phase 3  審查修改     比對 flowchart，逐一檢查
Phase 4  抽出 refs    把 prompt templates 放 references/
Phase 5  跑測試       開新 session，用真素材驗證
Phase 6  打包分發     放進 repo，clone 即用
```

**一句話總結**：Skill 就是「把你跟 AI 合作過一次的好經驗，變成可以重複使用的操作手冊」。

---

## 附：Claude Code vs Codex CLI 對照

| | Claude Code | Codex CLI |
|---|---|---|
| 建立 skill | `/skill-creator` (plugin) | `$skill-creator` (built-in) |
| Skill 位置 | `.claude/skills/<name>/SKILL.md` | `.codex/skills/<name>/SKILL.md` |
| 打包分發 | 放進 repo 即可 | `$plugin-creator`（自動包成 plugin） |
| Eval 測試 | `evals/evals.json` + benchmark | Skill Creator V2（parallel A/B 盲測） |
| 安裝指令 | `/plugin install` | `$plugin install` |
