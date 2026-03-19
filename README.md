# prd-writer

A Claude Code skill for the full product planning lifecycle — from fuzzy opportunity exploration all the way to PRD drafting, user story breakdown, metrics definition, competitive analysis, and roadmap planning.

## What It Does

Install this skill and Claude will:

* **探索機會**：還不確定問題值不值得解時，引導你從訊號整理 → 根本原因分析 → HMW 問題定義，輸出 Opportunity Brief
* **解法往回拆**：帶著解法假設進來（「老闆說要做 X」「我想做 Y 功能」）時，拆解解法背後的問題假設，確認問題定義正確再往前走
* **評估機會**：用結構化框架評估問題的真實性、影響範圍、策略連結，幫你決定值不值得投資
* **寫 PRD**：問診式引導，逐章確認背景、目標、使用者、功能需求，自動標記不完整的章節
* **拆 User Story**：強制每則 Story 有 who/what/why + Acceptance Criteria，不完整就停下來提示
* **定義 Metrics**：確保每個指標有分子、分母、時間窗口、追蹤方式，不允許只寫定性目標
* **競品分析**：只用你提供的資料，不捏造功能，缺資料就標記 [需補充來源]
* **Roadmap 規劃**：先問清楚 Now/Next/Later 各代表幾個月，再排優先級
* **build\_report**：跑完規劃流程後輸出 Markdown + HTML + PDF 三種格式的產品規劃報告，含完成度評分

## Installation

```
# 全域安裝（所有專案都能用）
cp -r prd-writer ~/.claude/skills/

# 或只在特定專案使用
cp -r prd-writer /your-project/.claude/skills/
```

重啟 Claude Code 後生效。

## Usage

直接在對話裡說你要做什麼，skill 會自動判斷你在哪個階段：

```
# 模糊探索階段（不確定問題是什麼）
這個問題值不值得解？
用戶一直反映 X，但我不知道根本原因
幫我探索這個機會
幫我定義問題 / HMW
老闆說要做 X，但我想確認問題定義
我想做 Y 功能，幫我確認方向對不對

# 規劃輸出階段（已確定要做什麼）
寫PRD：幫我寫一個會員登入功能的 PRD
拆 Story：把這個功能拆成 user story
定義指標：這個功能的 KPI 應該怎麼定義
競品分析：幫我分析一下這幾個競品
Roadmap：幫我排這季的產品規劃
產出報告：build report
```

## Trigger Conditions

**會觸發：**

* 探索機會 / 問題定義 / HMW / 這個值不值得做
* 老闆說要做 X / 我想做 Y 功能，確認方向
* 寫PRD / 需求文件 / feature spec / 寫需求
* 拆user story / 定義KPI / 定義指標
* 競品分析 / roadmap / 產品規劃
* build report / 產出報告 / 輸出文件

**不會觸發：**

* 一般產品閒聊（「你覺得這個 idea 好嗎？」）
* 技術架構討論（「這個功能的資料庫要怎麼設計？」）
* Code review

## How It Works

每次對話開始，skill 會先判斷你在哪個階段：

```
模糊階段（問題還不清楚）
  └→ 問題定義流程：訊號收集 → 根本原因 → HMW
  └→ 機會評估流程：真實性 → 影響範圍 → 策略連結
  └→ 輸出 Opportunity Brief
       └→ Brief 的內容自動銜接 PRD 問診，不需重填

清楚階段（已知要做什麼）
  └→ 直接進 PRD / Story / Metrics / Roadmap 流程
```

## File Structure

```
prd-writer/
├── SKILL.md                          # 入口，含階段判斷、觸發條件與路由邏輯
├── README.md                         # 本文件
├── references/
│   ├── problem-definition.md         # 訊號收集 → 根本原因分析 → HMW 收斂
│   ├── opportunity-sizing.md         # 機會評估框架（真實性/影響範圍/策略連結）
│   ├── solution-first.md             # 解法假設拆解 → 還原問題陳述 → 判斷路徑
│   ├── opportunity-brief.md          # Opportunity Brief 模板與銜接邏輯
│   ├── prd-template.md               # PRD 問診流程 + 各章節完成門檻
│   ├── user-story-guide.md           # User Story 格式、拆解方法、完成標準
│   ├── metrics-hypothesis.md         # 指標假設聲明：OKR 連結、護欄邏輯
│   ├── metrics-guide.md              # 四層指標框架、量測定義、完成標準
│   ├── competitive-analysis.md       # 競品分析框架與輸出格式
│   ├── roadmap-guide.md              # Now/Next/Later 框架與排序方法
│   ├── build-report.md               # 報告產生流程、品質評分算法、HTML 填充規則
│   ├── anchor.md                     # 跨階段 context 共享與版本規則
│   └── status-snapshot.md            # 狀態快照格式與品質門檻
└── assets/
    ├── prd-blank.md                  # 空白 PRD 模板
    ├── report-template.md            # Markdown 報告模板
    └── report-template.html          # HTML 報告模板（側邊導覽、進度條、高亮）
```

## Design Principles

**從哪裡開始都接得住**
不假設你已經知道要做什麼。模糊的機會訊號和明確的功能需求，都有對應的流程。

**問診優先，不填空**
不論哪個階段，填寫前先問對應的問題，不知道的標記 [待確認] 並列入開放問題，不猜測。

**完成標準明確**
每則 Story、每個 Metric、每個 PRD 章節都有 ✅/⚠️/❌ 三級評分門檻，build\_report 用這些門檻計算真實完成度。

**不捏造**
競品分析只用你提供的資料；機會評估不幫你決定「值不值得做」，只整理證據；架構細節不確定時標記 [待工程確認]。

**按需載入**
各 reference 文件只在對應任務觸發時載入，不佔用多餘 context。

## Changelog

* v0.1：PRD / User Story / Metrics 基本流程
* v0.2：觸發精準度優化、Gotchas 強化
* v0.3：各 reference 加入完成標準；build\_report 改為品質評分算法
* v0.4：新增競品分析、Roadmap 指南；HTML 填充邏輯補完
* v0.5：新增模糊探索前置階段（problem-definition / opportunity-sizing / opportunity-brief）；SKILL.md 加入階段判斷層
* v0.6：新增 solution-first.md，補足「帶著解法假設進來」的入口路徑
