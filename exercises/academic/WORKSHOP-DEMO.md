# Workshop Demo Script: Academic Chapter Draft Pipeline

**對應 Slide**: 30（場景介紹）→ 31（Interactive Flowchart）
**時間**: ~15 分鐘 live demo + 學生跟做
**素材位置**: `exercises/academic/`
**Demo 產出參考**: `exercises/academic/_demo-outputs/`

---

## 開場（1 分鐘）

> 你是 KCL 碩士生，deadline 三週後，手上有四份散亂素材，要交一份 Chapter 2 給指導教授。
> 我們要用 Claude Code 把這四份素材變成一份可送審的草稿。
> AI 是你的**研究助理**，不是你的**作者**。

打開 Slide 31 的 Interactive Flowchart，讓學生看到完整 pipeline：

- 🟠 橘色節點 = 你（學生）負責的決策和判斷
- 🟣 紫色節點 = AI 負責的執行和產出
- 紅色虛線 = 失敗路徑（做錯了怎麼回頭）

---

## Step 1 · 啟動（30 秒）

在 terminal 輸入：

```bash
cd exercises/academic/
claude
```

第一句話：

```
你能看到這個目錄裡有什麼嗎？列出完整結構。
```

**教學重點**：讓 Claude 先列出目錄，確認 4 份素材都在。不要跳步驟。

---

## Step 2 · 探索素材全覽（2 分鐘）

Claude 會自動讀取所有檔案，回報：
- 目錄樹
- 每份素材的角色（論文、引用、問卷、大綱）
- 建議 pipeline 順序

**現場互動**：問學生「看到 Claude 列出來的結構，有什麼跟你想像不一樣的嗎？」

確認方向後說：「好，開始 Step 01。」

---

## Step 3 · 🤖 文獻比較表（3 分鐘）

貼入 prompt（或讓 skill 自動帶入）：

```
讀 lit-review/samples/ 裡的三篇論文，產出 comparison_table.md。

表格欄位：論文 | 研究問題 | 方法 | 樣本規模 | 主要發現 | 局限

不確定的填 [unclear]，不要猜。完成後告訴我哪篇最支撐我的論點。
```

**等 Claude 產出後**，打開 `comparison_table.md` 展示給學生看。

**教學重點**：
- 指出表格裡的 `[unclear]` — 「這就是 AI 的誠實標記」
- 指出「最支撐論點」的分析 — 「AI 可以幫你判斷，但最終決定是你的」

---

## Step 4 · 👤 校對表格（1 分鐘）

**現場示範**校對方法：

```
paper1 的主要發現，你是從第幾段提取的？
```

讓 Claude 定位原文。展示給學生：「你不需要每格都查，只抽查你論文最依賴的欄位。」

**如果發現錯誤**（教學機會）：
```
comparison_table.md 裡 paper2 的樣本規模寫錯了，重新產一次。
```

→ 展示 Slide 31 的 failure path：Step 4 → Step 3 紅色虛線。

---

## Step 5 · 🤖 整理引用格式（2 分鐘）

```
讀 references/refs_messy.txt，統一成 APA 7 格式，產出 refs_clean.md。
缺的欄位標 [缺]，不要猜。最後列出重複和格式異常的條目。
```

**展示重點**：
- 找到的 2 筆重複引用（Chen 2024 出現兩次，Brynjolfsson 2017 出現兩次）
- 2 筆格式異常（只有 URL 沒有書目）
- 「這些是你手動不太會發現的問題」

---

## Step 6 · 🤖 挖關鍵數字（2 分鐘）

```
讀 survey-analysis/survey.csv。
我的論點：AI 工具使用頻率越高，學術誠信疑慮也越高。
找 3 個支撐這個論點的趨勢。如果有反例也要標出來。
```

**這是 demo 的高光時刻。** Claude 會發現：

> ⚠️ **反例**：freq_use_ai 和 concern_integrity 的相關係數 r = -0.039（幾乎為零）。
> 如果論點是「用越多就越擔心」，**數據不支持**。

**教學重點**：「AI 沒有幫你圓說法。它告訴你數據不支持你的假設，然後建議你調整論點方向。這就是為什麼我們說 AI 是研究助理不是作者 — 它不會幫你作假。」

讓學生翻 Slide 31，點 Step 7 節點，看 failure path。

---

## Step 7 · 👤 評估數字（1 分鐘）

問學生：「你覺得 Claude 建議的論點調整方向對嗎？」

> 建議：不要主張「用越多→越擔心」的因果關係，而是主張兩者都是「判斷力落差」的症狀。

「如果你接受這個調整，我們繼續。如果不接受，可以讓 Claude 帶著不同的角度重新分析。」

---

## Step 8 · 🤖 撰寫草稿（3 分鐘）

```
整合 outline.md + comparison_table.md + refs_clean.md + key_stats.md，
擴寫成 chapter2_draft.md。

每段第一句是 topic sentence。引用標 [needs citation: 關鍵字]。
統計數字附原始欄位名和樣本數。不要編造。
```

**等產出後展示**：
1. 打開 `chapter2_draft.md`，展示 `[needs citation: Chen AI adoption UK SMEs]` 標記
2. 展示最後的「Weakest Transitions」自評
3. 「Claude 自己告訴你哪裡最弱 — 這些是你需要花時間改的地方」

---

## Steps 9-10 · 👤 補引用 + 改寫（1 分鐘，口頭帶過）

> 最後兩步是你的事，不是 AI 的事。
>
> Step 9：把每個 `[needs citation]` 換成 refs_clean.md 裡的真實引用。找不到的標 `[missing ref]` — 這是你後續需要補新文獻的地方。
>
> Step 10：用自己的話重寫每一段。測試方法：你能不能不看草稿，向 supervisor 口頭說明每一段在講什麼？能的話才算通過。

---

## 收尾（1 分鐘）

回到 Slide 31 flowchart，總結：

> 這條 pipeline 的核心不是「讓 AI 寫論文」。
> AI 處理了你不需要親自動手的事 — 格式轉換、CSV 分析、初稿框架。
> 你的時間集中在只有你能做的事：**判斷、理解、改寫**。

---

## 學生跟做指引

學生跟做時的注意事項：

1. **不要複製 `_demo-outputs/`** — 那是 demo 參考，你要自己跑一次
2. **換成你自己的論點方向** — 改 Step 6 prompt 裡的「我的論點」
3. **Step 4 和 Step 7 不能跳** — 這是品質把關，跳過等於放棄學術誠信
4. **遇到 failure path 是正常的** — 第一次 prompt 不完美很正常，重來就好

### 如果學生已安裝 skill

```bash
cd exercises/academic/
claude
```

Claude 會自動載入 `academic-chapter-draft` skill 並引導整個 pipeline。

### 如果沒裝 skill

手動依照上面的 prompt 一步步貼入即可，效果一樣。

---

## Demo 產出參考

完整的 demo 產出放在 `_demo-outputs/` 目錄下：

| 檔案 | 對應步驟 | 說明 |
|------|----------|------|
| `comparison_table.md` | Step 3 | 3 篇論文比較表 + 最支撐論點分析 |
| `refs_clean.md` | Step 5 | 14 筆 APA 7 引用 + 重複/異常標記 |
| `key_stats.md` | Step 6 | 3 個趨勢 + 反例（r=-0.039） |
| `chapter2_draft.md` | Step 8 | 4 節完整草稿 + `[needs citation]` 標記 |

學生可以在跟做完後，比對自己的產出和 demo 產出的差異。
