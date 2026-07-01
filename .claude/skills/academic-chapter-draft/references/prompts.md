# Prompt Templates

Each AI step has a prompt template below. Read the relevant section when executing that step.
Adapt the thesis direction and column names to match the student's actual materials.

---

## Step 2 Prompt

```
你是學術助理。我在 exercises/academic/ 目錄下。請：
1. 列出所有子目錄和檔案的完整結構
2. 對每份素材說明：類型、大概內容、字數/筆數
3. 告訴我這四份素材各自扮演什麼角色（lit-review/ 是待比較的論文，不是我要寫的東西）
4. 建議一條從這四份素材到 chapter2_draft.md 的 pipeline 步驟順序，並說明每步的輸入和輸出是什麼

確認你能看到以下四份素材再開始：lit-review/samples/、references/refs_messy.txt、survey-analysis/survey.csv、dissertation/outline.md
```

---

## Step 3 Prompt

```
你是學術助理。讀 lit-review/samples/ 裡的論文，產出 comparison_table.md。

表格欄位（每篇論文一行）：
| 論文 | 研究問題 | 方法 | 樣本規模 | 主要發現 | 局限 |

要求：
- 不確定的欄位填 [unclear]，不可自行猜測或填補
- 主要發現只寫最重要的一點（一句話）
- 局限只寫論文自己承認的局限，不是你的評論
- 完成後告訴我：哪一篇論文最能支撐我的核心論點？為什麼？
```

Adapt "核心論點" to the student's actual thesis from outline.md.

---

## Step 5 Prompt

```
你是學術助理。讀 references/refs_messy.txt，把所有引用統一整理為 APA 7 格式，產出 refs_clean.md。

要求：
- 依第一作者姓氏字母順序排列
- 缺失欄位（如出版年、期刊名、頁碼、DOI）一律標 [缺]，禁止自行猜測或填補
- 保留原始資訊，不得刪除任何引用
- 最後附上一個「⚠️ 可能重複的引用」清單（相同作者+年份或相同標題）
- 最後附上一個「❓ 格式異常」清單（只有 URL 沒有書目資訊的條目）

完成後告訴我：這份清單裡有幾篇跟我的研究主題相關的文獻？
```

---

## Step 6 Prompt

```
你是資料分析助理。讀 survey-analysis/survey.csv。

我的論點方向：[從 outline.md 提取核心論點]

請分析相關欄位並找出 3 個最能支撐這個論點的關鍵趨勢。

對每個趨勢說明：
1. 具體數字（平均值、比例、或相關性）
2. 支撐這個數字的樣本數
3. 這個數字為什麼能支撐我的論點（一句話）

產出 key_stats.md。如果發現反例（與論點矛盾的群體），也要標出來。
```

---

## Step 8 Prompt

```
你是學術寫作助理。請讀以下四份檔案：
- dissertation/outline.md（Chapter 2 大綱和核心論點）
- comparison_table.md（論文比較表）
- refs_clean.md（APA 7 引用清單）
- key_stats.md（關鍵數字）

依照 outline.md 的 Chapter 2 結構，擴寫成完整學術段落，產出 chapter2_draft.md。

寫作要求：
1. 每個段落第一句是 topic sentence（清楚說明這段的論點）
2. 引用文獻時標 [needs citation: 論文標題關鍵字]，讓我知道應該填哪篇
3. 引用統計數字時，附原始欄位名和樣本數，例如：（freq_use_ai 平均 3.6，n=100）
4. 不得自行編造數字或引用不在 refs_clean.md 裡的文獻
5. 每個子章節結束後加一行 [END OF SECTION X]，方便核對大綱

完成後說明：哪些地方你認為論點銜接最薄弱，需要補充說明？
```
