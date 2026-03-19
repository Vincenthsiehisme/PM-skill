---
name: prd-writer
description: |
  Use for structured product planning output: PRD drafting, user story breakdown, KPI/metrics definition, competitive analysis, or roadmap planning. Trigger on: 寫PRD, 需求文件, 拆user story, 定義KPI, 定義指標, 競品分析, roadmap, 產品規劃, feature spec, 寫需求.

  **也在以下模糊階段觸發**：「這個問題值不值得解」「幫我探索這個機會」「我有個想法但不知道從哪開始」「問題定義書」「HMW」「這個值不值得做」「用戶一直反映 X 但我不知道根本原因」。

  DO NOT trigger for: general product chat, technical architecture discussion, code review, or one-line questions about product concepts.
---

# PRD Writer（含模糊探索前置階段）

---

## Step 0：判斷你現在在哪個階段

**每次對話開始，先問這一題（只問一次）：**

> 「你現在對這個問題/功能的清楚程度是？
> A）還不確定問題值不值得解，或根本問題是什麼
> B）問題方向清楚，要開始規劃和輸出文件」

| 回答 | 進入路徑 |
|------|---------|
| A（模糊階段） | → 進入 **模糊探索流程**（見下方） |
| B（清楚階段） | → 跳過模糊探索，進入 **PRD Writer 主流程** |
| 沒說清楚 | → 直接進 A，模糊探索完再跳 B |

**例外：若使用者說「寫 PRD」「拆 user story」「定義 KPI」等明確任務，直接進 B，不問。**

---

## 模糊探索流程（Fuzzy Front End）

> 適用：問題還不清楚、機會還沒評估、不知道從哪開始。

### 判斷子路徑

| 使用者狀態 | 載入的 reference |
|-----------|----------------|
| 「有訊號但不知道問題是什麼」 | `references/problem-definition.md` |
| 「知道問題但不確定值不值得解」 | `references/opportunity-sizing.md` |
| 「我要做 X 功能」「老闆說要做 Y」（帶著解法進來） | `references/solution-first.md` |
| 「老闆指派方向，想確認框架」 | `references/solution-first.md` |
| 「完全不確定，從頭開始」 | 先 `problem-definition.md`，完成後 `opportunity-sizing.md` |

### 模糊探索的輸出

兩個子路徑都有各自的中間產出（Problem Definition Canvas / Opportunity Snapshot），最終收斂到：

**Opportunity Brief** → 載入 `references/opportunity-brief.md`

### 從模糊探索跳到 PRD Writer

Opportunity Brief 完成後，詢問 PM：

> 「Opportunity Brief 完成了。要繼續開始寫 PRD 嗎？Brief 的問題陳述和目標用戶可以直接回答 prd-writer 的前兩個問診問題，不需要重填。」

若 PM 說是 → 帶著 Brief 的資訊進入下方的 **PRD Writer 主流程**，自動跳過重複的問診問題。

---

## PRD Writer 主流程

### 1. 檢查 Anchor

搜尋對話中是否存在 Anchor Statement（格式為 `## ✅ Anchor vN`）。

- **有 Anchor** → 讀取版本號最高的那個，帶入後續所有任務
- **沒有 Anchor** → 正常從問診開始，第一個階段完成後初始化 Anchor v1

**如果剛從模糊探索流程過來**，把 Opportunity Brief 的內容視為 Anchor 的初始資料，不要重問已有答案的問題。

### 2. 確認任務類型，載入對應 reference

| 任務 | 載入的文件 |
|------|-----------|
| 寫 PRD | `references/prd-template.md` |
| 拆 User Story / Ticket | `references/user-story-guide.md` |
| 定義 Metrics / KPI | 先 `references/metrics-hypothesis.md`，問診後 `references/metrics-guide.md` |
| 競品分析 | `references/competitive-analysis.md` |
| Roadmap 規劃 | `references/roadmap-guide.md` |
| 不確定 / 全面規劃 | 載入全部五個 |
| build_report | `references/build-report.md` |
| 每次輸出章節後 | `references/status-snapshot.md` |
| 每個階段完成後 | 輸出更新的 Anchor（見 `references/anchor.md`） |
| 首次對話、無 Anchor 時 | `references/anchor.md` |

### 3. 詢問 context（只在使用者未提供時）

若使用者只說「幫我寫 PRD」但沒說功能是什麼，先問一題：

> 「這個功能主要解決什麼問題？目標使用者是誰？」

拿到答案後直接進入 prd-template 的問診流程，不要再問第二輪。

---

## Output Principles

- **語言**：繁體中文，除非使用者要求英文
- **格式**：Markdown，可直接貼到 Notion / Confluence / GitHub
- **長度**：PRD 完整展開；User Story 一個功能 3–5 則；Metrics 明確列出分子分母
- **不捏造**：不確定的架構細節用 `[待確認]` 標記

### 輸出後停等原則（Stop-and-Align）

每次輸出章節、Story 或 Metric 後，必須：

1. 輸出狀態快照（見 `references/status-snapshot.md`）
2. 等待 PM 回應 A / B / C
3. 根據回應決定下一步

**絕對不做**：輸出快照後自動繼續；發現缺口後自動追問；把「有缺口」當必須立刻填補的訊號。

**PM 的回應決定節奏，不是 Claude 的判斷。**

---

## Gotchas

**模糊探索階段**
- **把解法當問題**：PM 說「我想做推薦功能」→ 先問「推薦功能要解決什麼問題？」，不接受解法作為問題陳述
- **問題太大無法操作**：「用戶不夠黏」是症狀，不是問題定義。追問到具體行為場景
- **跳過 evidence 直接結論**：問題的嚴重程度必須有數據或訪談佐證
- **HMW 太廣或太窄**：「HMW 讓用戶更快樂」太廣；HMW 應該有具體情境但仍有設計空間

**PRD 撰寫階段**
- **PRD 滑移成 Tech Spec**：使用者提到「資料庫」「API」時，強制標記 `[待工程確認]`，PRD 開頭加「本文件為產品需求，技術實作另見 Tech Spec」
- **Metrics 沒有分母**：「提升 30% 點擊率」不是 KPI。分子、分母、時間窗口都填完才輸出
- **User Story 沒有 AC**：每則 Story 至少一條 Given/When/Then 才算完成
- **競品分析變通用摘要**：只用使用者提供的資料，不足標記 `[需補充來源]`，不捏造
- **Roadmap 沒定義 horizon**：先問 Now/Next/Later 各幾個月，再排優先級

---

## Reference Files

**模糊探索（Fuzzy Front End）**
- `references/problem-definition.md` — 訊號收集 → 根本原因分析 → HMW 收斂
- `references/opportunity-sizing.md` — 機會評估框架（真實性 / 影響範圍 / 策略連結）
- `references/solution-first.md` — 解法假設拆解 → 還原問題陳述 → 判斷是否需重新定義
- `references/opportunity-brief.md` — Opportunity Brief 模板、完成門檻、銜接 prd-writer 邏輯

**PRD 撰寫**
- `references/prd-template.md` — PRD 問診流程 + 各章節完成門檻
- `references/user-story-guide.md` — User Story 格式、拆解方法、覆蓋度檢查
- `references/metrics-hypothesis.md` — 指標假設聲明：OKR 連結、假設透明、護欄邏輯
- `references/metrics-guide.md` — 四層指標框架、量測定義、baseline 建立
- `references/competitive-analysis.md` — 競品分析框架、功能矩陣、差異化洞察
- `references/roadmap-guide.md` — Now/Next/Later 框架、Impact vs Effort 矩陣
- `references/build-report.md` — 受眾校準、品質評分、三種格式輸出
- `references/anchor.md` — Anchor 機制：跨階段共享 context、版本號規則
- `references/status-snapshot.md` — 狀態快照格式與品質門檻

**Assets**
- `assets/prd-blank.md` — 空白 PRD 模板
- `assets/report-template.md` — Markdown 報告模板
- `assets/report-template.html` — HTML 報告模板

---

## 版本迭代

PRD 需要更新時（使用者說「更新 PRD」「這個要改」「工程說做不到」）：

1. 先問清楚變更範圍
2. 版本號遞增：v0.1 → v0.2
3. 標記變更：`> 📝 v0.2 更新：[一句話說明]`
4. 保留歷史：舊內容加刪除線 `~~舊內容~~`，新內容寫在下方
5. 開放問題同步：已解決的標記 `✅ 已解決`
6. 更新 Anchor：加入改動記錄，受影響欄位加 `←更新`

**版本狀態**：`草稿` → `審閱中` → `確認` → `v[N] 修訂`

**Gotcha**：使用者說「改一下」但沒說改哪裡 → 先列章節摘要，問「要更新哪個章節？」，不猜。
