---
name: prd-writer
description: 'Use for structured product planning output: PRD drafting, user story breakdown, KPI/metrics definition, competitive analysis, or roadmap planning. Trigger on: 寫PRD, 需求文件, 拆user story, 定義KPI, 定義指標, 競品分析, roadmap, 產品規劃, feature spec, 寫需求. DO NOT trigger for: general product chat, technical architecture discussion, code review, or one-line questions about product concepts. Trigger only when the user needs a structured document or framework as output.'
---

# PRD Writer

A skill for writing PRDs, breaking down user stories, and defining metrics — optimized for a solo PM working with evolving system architecture context.

## How to Use This Skill

1. **對話開始時，先檢查 Anchor**：

   搜尋對話中是否存在 Anchor Statement（格式為 `## ✅ Anchor vN`）。
   - **有 Anchor** → 讀取版本號最高的那個，帶入後續所有任務
   - **沒有 Anchor** → 正常從問診開始，第一個階段完成後初始化 Anchor v1

2. **問清楚任務類型**，然後載入對應的 reference 文件：

| 任務 | 載入的文件 |
|------|-----------|
| 寫 PRD | `references/prd-template.md` |
| 拆 User Story / Ticket | `references/user-story-guide.md` |
| 定義 Metrics / KPI | 先載入 `references/metrics-hypothesis.md`，問診完成後載入 `references/metrics-guide.md` |
| 競品分析 | `references/competitive-analysis.md` |
| Roadmap 規劃 | `references/roadmap-guide.md` |
| 不確定 / 全面規劃 | 載入全部五個 |
| build_report | `references/build-report.md` |
| 每次輸出章節內容後 | `references/status-snapshot.md` |
| 每個階段完成後 | 輸出更新的 Anchor（見 `references/anchor.md`） |

2. **詢問系統架構 context**（見下方 Context Gathering）

3. 輸出對應格式的文件

---

## Context Gathering

**只在使用者沒有主動提供時才問，避免與 prd-template 的問診問題重複。**

載入 prd-template.md 後，問診問題由各章節自己負責。
這裡只處理「連功能是什麼都還不清楚」的初始狀態。

若使用者只說「幫我寫 PRD」但沒說功能是什麼，先問這一題就好：
> 「這個功能主要解決什麼問題？目標使用者是誰？」

拿到答案後直接進入 prd-template 的問診流程，不要再問第二輪。

---

## Output Principles

- **語言**：用繁體中文輸出，除非使用者明確要求英文
- **格式**：Markdown，可直接貼到 Notion / Confluence / GitHub
- **長度**：PRD 完整展開；User Story 一個功能對應 3–5 則；Metrics 明確列出分子分母
- **不要捏造**：架構細節不確定時，用 `[待確認]` 標記，不要假設

---

## Gotchas

- **PRD 會自動滑移成 Tech Spec**：只要使用者提到「資料庫」「API」「架構」，Claude 就會開始寫技術設計。必須在輸出時強制標記 `[待工程確認]`，並在 PRD 開頭加一行「本文件為產品需求，技術實作另見 Tech Spec」
- **Metrics 沒有分母就輸出了**：「提升 30% 點擊率」不是 KPI。每次輸出 metrics 之前，必須先確認分子、分母、時間窗口都填完，缺一不輸出
- **User Story 沒寫 Acceptance Criteria 就停下來了**：Claude 寫完 Story 後容易直接換下一條。必須強制每則 Story 至少有一條 Given/When/Then 才算完成，不能只有 Story 本體
- **競品分析變成通用知識摘要**：沒有使用者提供的具體資料時，Claude 會用訓練資料捏造競品功能描述。競品分析只能用使用者提供的資料，不足的地方標記 `[需補充來源]`，不捏造
- **Roadmap 沒定義 horizon 就輸出**：Now / Next / Later 的時間範圍如果使用者沒說，Claude 會自己猜（通常猜錯）。必須先問清楚三個 horizon 各代表幾個月，再開始排優先級

---

## Reference Files

按需載入：
- `references/prd-template.md` — PRD 問診流程 + 各章節完成門檻
- `references/user-story-guide.md` — User Story 格式、拆解方法、覆蓋度檢查
- `references/metrics-hypothesis.md` — 指標假設聲明：OKR 連結、假設透明、護欄選擇邏輯（定義指標前先跑）
- `references/metrics-guide.md` — 四層指標框架、量測定義、baseline 建立（假設聲明完成後使用）
- `references/competitive-analysis.md` — 競品分析框架、功能矩陣、差異化洞察
- `references/roadmap-guide.md` — Now/Next/Later 框架、Impact vs Effort 矩陣
- `references/build-report.md` — 受眾校準、品質評分、三種格式輸出、HTML 填充規則
- `references/anchor.md` — Anchor 機制：跨階段共享 context、版本號規則、各欄位對應的問診跳過邏輯
- `references/status-snapshot.md` — 狀態快照格式與品質門檻對照表
- `assets/prd-blank.md` — 空白 PRD 模板
- `assets/report-template.md` — Markdown 報告模板
- `assets/report-template.html` — HTML 報告模板（側邊導覽、進度條、highlight）

---

## 版本迭代

PRD 需要更新時，使用者會說「更新 PRD」「這個要改」「工程說做不到」等。

**處理流程：**

1. **先問清楚變更範圍**：是改一個章節，還是整份 PRD 需要重新審視？
2. **版本號遞增**：v0.1 → v0.2，在文件資訊處更新版本號和日期
3. **標記變更**：在被修改的章節開頭加上 `> 📝 v0.2 更新：[一句話說明改了什麼]`
4. **保留歷史**：不要直接覆蓋舊內容，改為用刪除線標記：`~~舊內容~~`，新內容寫在下方
5. **開放問題同步**：若變更是因為某個開放問題有了答案，把該問題標記為 `✅ 已解決` 並說明結論

**版本狀態定義：**
- `草稿`：初版，尚未送審
- `審閱中`：已送給 stakeholder 審閱
- `確認`：已拍板，進入開發
- `v[N] 修訂`：開發期間因需求變更而修改

**Gotcha：** 使用者說「改一下」但沒說改哪裡時，先列出目前文件的章節摘要，問「你要更新哪個章節？」，不要猜。
