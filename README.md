# prd-writer

A Claude Code skill for structured product planning — PRD drafting, user story breakdown, metrics definition, competitive analysis, and roadmap planning.

## What It Does

Install this skill and Claude will:

- **寫 PRD**：問診式引導，逐章確認背景、目標、使用者、功能需求，自動標記不完整的章節
- **拆 User Story**：強制每則 Story 有 who/what/why + Acceptance Criteria，不完整就停下來提示
- **定義 Metrics**：確保每個指標有分子、分母、時間窗口、追蹤方式，不允許只寫定性目標
- **競品分析**：只用你提供的資料，不捏造功能，缺資料就標記 [需補充來源]
- **Roadmap 規劃**：先問清楚 Now/Next/Later 各代表幾個月，再排優先級
- **build_report**：跑完規劃流程後輸出 Markdown + HTML + PDF 三種格式的產品規劃報告，含完成度評分

## Installation

```bash
# 全域安裝（所有專案都能用）
cp -r prd-writer ~/.claude/skills/

# 或只在特定專案使用
cp -r prd-writer /your-project/.claude/skills/
```

重啟 Claude Code 後生效。

## Usage

直接在對話裡說你要做什麼，skill 會自動觸發：

```
寫PRD：幫我寫一個會員登入功能的 PRD
拆 Story：把這個功能拆成 user story
定義指標：這個功能的 KPI 應該怎麼定義
競品分析：幫我分析一下這幾個競品
Roadmap：幫我排這季的產品規劃
產出報告：build report
```

## Trigger Conditions

**會觸發：**
- 寫PRD / 需求文件 / feature spec / 寫需求
- 拆user story / 定義KPI / 定義指標
- 競品分析 / roadmap / 產品規劃
- build report / 產出報告 / 輸出文件

**不會觸發：**
- 一般產品閒聊（「你覺得這個 idea 好嗎？」）
- 技術架構討論（「這個功能的資料庫要怎麼設計？」）
- Code review

## File Structure

```
prd-writer/
├── SKILL.md                          # 入口，含觸發條件與路由邏輯
├── README.md                         # 本文件
├── references/
│   ├── prd-template.md               # PRD 問診流程 + 各章節完成門檻
│   ├── user-story-guide.md           # User Story 格式、拆解方法、完成標準
│   ├── metrics-guide.md              # 四層指標框架、量測定義、完成標準
│   ├── competitive-analysis.md       # 競品分析框架與輸出格式
│   ├── roadmap-guide.md              # Now/Next/Later 框架與排序方法
│   └── build-report.md               # 報告產生流程、品質評分算法、HTML 填充規則
└── assets/
    ├── prd-blank.md                  # 空白 PRD 模板
    ├── report-template.md            # Markdown 報告模板
    └── report-template.html          # HTML 報告模板（側邊導覽、進度條、高亮）
```

## Design Principles

**問診優先，不填空**
每個章節在填寫前先問對應的問題，不知道的標記 [待確認] 並列入開放問題，不猜測。

**完成標準明確**
每則 Story、每個 Metric、每個 PRD 章節都有 ✅/⚠️/❌ 三級評分門檻，build_report 用這些門檻計算真實完成度。

**不捏造**
競品分析只用使用者提供的資料；架構細節不確定時標記 [待工程確認]；Metrics 沒有 baseline 就明說待定。

**按需載入**
各 reference 文件只在對應任務觸發時載入，不會在每次對話都佔用 context。

## Changelog

- v0.1：PRD / User Story / Metrics 基本流程
- v0.2：觸發精準度優化、Gotchas 強化（從假設性警告改為真實失敗模式）
- v0.3：各 reference 加入完成標準；build_report 改為品質評分算法
- v0.4：新增競品分析、Roadmap 指南；HTML 填充邏輯補完；README 補齊
