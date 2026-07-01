---
name: academic-chapter-draft
description: >
  Transform scattered academic materials (papers, messy citations, survey CSV, outline) into a
  polished dissertation chapter draft through a 10-step pipeline. Use this skill whenever a user
  mentions "chapter draft", "dissertation writing", "literature review pipeline", "論文寫作",
  "學術寫作", "chapter 2", or has a working directory with lit-review/, references/, survey-analysis/,
  and dissertation/ folders. Also trigger when user says "academic pipeline", "paper to draft",
  or wants to combine multiple academic sources into a single chapter.
---

# Academic Chapter Draft Pipeline

Turn 4 types of scattered academic materials into a submission-ready chapter draft.
The pipeline has 10 steps — AI handles execution (steps 2, 3, 5, 6, 8), the student
handles every decision and quality gate (steps 1, 4, 7, 9, 10).

## Academic Integrity — Non-Negotiable

You are a **research assistant**, not an author. Three rules that override everything:

1. **Never fabricate** — citations, statistics, and findings come only from provided materials
2. **Mark uncertainty** — anything unverifiable gets `[unclear]`, `[needs citation]`, or `[missing ref]`
3. **Student rewrites** — the final draft must be in the student's own words and reasoning

## Expected Input

```
exercises/academic/
├── lit-review/samples/     # 1-5 paper files (.md or .pdf)
├── references/refs_messy.txt   # Messy citation list, mixed formats
├── survey-analysis/survey.csv  # Raw survey data
└── dissertation/outline.md     # Chapter outline + research question
```

Confirm all 4 directories exist before starting. If anything is missing, stop and ask.

## Pipeline — 10 Steps

### Step 1 · 👤 啟動 (Student)

Student opens terminal in the exercises/academic/ directory and starts Claude Code.
Your first action: list every file in the working directory so both you and the student
can see what's available.

### Step 2 · 🤖 探索素材全覽

Read every file in the working directory. Report:

1. Complete directory tree with file sizes
2. For each material: type, approximate content, word/row count
3. Role assignment — explain what each folder contributes to the pipeline
4. Proposed pipeline order with input→output for each step

**Wait for student confirmation** before proceeding.

Read `references/prompts.md` § "Step 2 Prompt" for the exact prompt template.

### Step 3 · 🤖 Step 01 — 文獻比較表

Read all papers in `lit-review/samples/`. Produce `comparison_table.md`:

| 論文 | 研究問題 | 方法 | 樣本規模 | 主要發現 | 局限 |

Rules:
- Uncertain fields → `[unclear]`, never guess
- Key findings = one sentence per paper, the single most important finding
- Limitations = only what the paper itself acknowledges, not your commentary
- After the table, state which paper best supports the student's thesis and why

Read `references/prompts.md` § "Step 3 Prompt" for the exact prompt template.

### Step 4 · 👤 校對表格準確性 (Student)

**Human checkpoint.** The student spot-checks 2-3 cells against the original papers.
Focus areas: key findings + limitations (highest error rate).

If inaccurate → return to Step 3 with more specific prompt.
If accurate → proceed to Step 5.

Offer to help locate source passages: "Want me to show you which paragraph I extracted this from?"

### Step 5 · 🤖 Step 02 — 整理引用格式

Read `references/refs_messy.txt`. Produce `refs_clean.md` in APA 7 format:

- Sort by first author surname alphabetically
- Missing fields → `[缺]` (never fill in guesses)
- Preserve all original entries, delete nothing
- Append: "⚠️ 可能重複的引用" list (same author+year or same title)
- Append: "❓ 格式異常" list (URL-only entries without bibliographic info)
- End with: count of AI/education-related references

Read `references/prompts.md` § "Step 5 Prompt" for the exact prompt template.

### Step 6 · 🤖 Step 03 — 挖關鍵數字

Read `survey-analysis/survey.csv`. Produce `key_stats.md`:

The student's thesis direction comes from `dissertation/outline.md`. Read it first to
understand the argument, then find 3 trends that support it.

For each trend, report:
1. Specific number (mean, proportion, or correlation)
2. Sample size supporting it
3. One sentence: why this number supports the thesis

Also flag counter-examples (data that contradicts the thesis) — hiding them is dishonest.

Read `references/prompts.md` § "Step 6 Prompt" for the exact prompt template.

### Step 7 · 👤 評估數字相關性 (Student)

**Human checkpoint.** Student judges whether key_stats.md numbers actually support Chapter 2's
core argument. This is a judgment call only the student can make.

If stats are irrelevant → return to Step 6 with refined thesis direction.
If stats are relevant → student marks which ones to include in the draft.

Offer: "If you're unsure whether a number is convincing, I can role-play as an examiner and challenge it."

### Step 8 · 🤖 Step 04 — 撰寫章節草稿

Read all 4 inputs:
- `dissertation/outline.md` (structure + thesis)
- `comparison_table.md` (literature evidence)
- `refs_clean.md` (citation pool)
- `key_stats.md` (quantitative evidence)

Produce `chapter2_draft.md`:

- Follow outline.md's chapter structure exactly
- Every paragraph starts with a topic sentence stating the paragraph's argument
- Citations marked as `[needs citation: keyword]` so student knows which paper to fill in
- Statistics include original column name and sample size: `(freq_use_ai mean 3.6, n=100)`
- Never invent numbers or cite sources not in refs_clean.md
- End each sub-section with `[END OF SECTION X]` for easy outline cross-checking
- Close with: "Weakest transitions" list — where argument flow is thinnest

Read `references/prompts.md` § "Step 8 Prompt" for the exact prompt template.

### Step 9 · 👤 補上真實引用 (Student)

Student replaces every `[needs citation: keyword]` with an actual APA 7 citation from
`refs_clean.md`. Entries with no matching source → `[missing ref]`.

Offer to help: "Want me to search refs_clean.md for papers that discuss [keyword]?"

### Step 10 · 👤 改寫成自己的字 (Student)

Student rewrites AI-generated paragraphs in their own voice. The test: can you explain
every sentence to your supervisor without looking at the draft?

If a paragraph's logic is unclear → offer to explain it in plain language first,
then let the student rewrite.

### ✅ Done — chapter2_draft.md

A complete chapter draft with:
- Real citations (student-verified)
- Survey data (from provided CSV only)
- The student's own argument and voice
- Clear `[missing ref]` markers for gaps to address later

## Failure Paths (Retry Loops)

| From | To | Trigger |
|------|----|---------|
| Step 4 | Step 3 | Table cells inaccurate |
| Step 7 | Step 6 | Statistics don't support thesis |
| Step 8 | Step 3 | Draft has insufficient literature support |
| Step 10 | Step 8 | Student can't understand a paragraph |

When retrying, explain what went wrong and what the refined prompt changes.

## Common Mistakes

1. **Fabricating citations** — the #1 failure mode. Only reference materials actually provided.
2. **Filling in [unclear] with guesses** — uncertainty markers exist for a reason.
3. **Skipping human checkpoints** — steps 4, 7, 9, 10 require student judgment, not AI.
4. **Generic topic sentences** — each paragraph opener must state a specific claim, not "This section discusses..."
5. **Hiding counter-evidence** — if the data contradicts the thesis, flag it. Academic honesty > convenience.
