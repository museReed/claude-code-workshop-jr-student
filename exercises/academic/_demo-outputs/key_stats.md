# Key Statistics from Survey Data

**Source:** `survey-analysis/survey.csv` · n=100 KCL postgraduate students
**Thesis direction:** AI tool adoption frequency alone does not predict capability — the gap between "using tools" and "building judgement" is the core tension.

---

## Trend 1: 使用頻率高但信任度低 — 工具能力 ≠ 判斷能力

- 整體 AI 使用頻率偏高（freq_use_ai 平均 3.54/5，n=100）
- 但信任度顯著偏低（trust_ai_output 平均 2.84/5，n=100）
- 高頻使用者（freq ≥ 4，n=50）的信任度（2.88）與低頻使用者（freq ≤ 3，n=50）的信任度（2.80）幾乎一樣
- 只有 20% 的學生信任度 ≥ 4

**支撐論點：** 學生大量使用 AI 工具，但使用頻率並未讓他們更信任 AI 的產出。這正是「工具採用 ≠ 能力建構」的微觀證據 — 用得多不代表判斷得準。(freq_use_ai mean 3.54 vs trust_ai_output mean 2.84, n=100)

## Trend 2: 學術誠信疑慮普遍且與使用頻率無關

- 全體學生 concern_integrity 平均 3.48/5（n=100）
- 高頻使用者（freq ≥ 4）中 44% 有高疑慮（concern ≥ 4）
- 低頻使用者（freq ≤ 3）中 48% 有高疑慮（concern ≥ 4）
- 兩組幾乎無差異，相關性極弱（r = -0.039）

**支撐論點：** 誠信疑慮不是「用太多才擔心」——它是結構性的。無論使用頻率高低，約半數學生都擔心學術誠信問題。這支撐「需要系統性的判斷力訓練，而非單純限制使用」的政策建議。(concern_integrity ≥ 4 比例: high-freq 44% vs low-freq 48%, n=100)

## Trend 3: 質性回應揭示「判斷力落差」

Open responses 中反覆出現的主題：
- **依賴性擔憂**（9/100）："I rely on it too much for first drafts and my own writing has gotten lazy."
- **剽竊焦慮**（6/100）："Worry about plagiarism detection picking it up."
- **界線模糊**（5/100）："Hard to know where to draw the line between help and doing it for me."
- **跳過閱讀的誘惑**（4/100）："It can't replace reading the actual paper, but tempted to skip reading."
- **被懷疑作弊**（4/100）："Worried my supervisor thinks I'm cheating even when I just use it for grammar."

**支撐論點：** 學生自己意識到了「工具能力」和「判斷能力」之間的落差 — 他們知道自己在用，但不確定自己是否「真的學到東西」。這與 Hoffmann et al. (2024) 發現的「younger managers report higher confidence but lower accuracy」形成呼應。

---

## ⚠️ 反例（Counter-evidence）

**直接相關性不存在：** freq_use_ai 和 concern_integrity 的相關係數 r = -0.039（幾乎為零）。如果論點是「用越多就越擔心」，數據不支持。

**建議論點調整：** 不要主張「使用頻率→誠信疑慮」的因果關係，而是主張兩者都是「判斷力落差」的症狀 — 學生大量使用卻不信任，同時普遍擔心但無法自行劃界線。這是 Hoffmann 的 capability-based view 在教育場景的延伸。
