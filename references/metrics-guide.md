# Metrics Guide

## 四層指標框架

### 1. 北極星指標（North Star Metric）
整個產品只有一個。反映核心價值交付。
範例：「每週活躍創作者數」、「每月完成訂單數」

### 2. 驅動指標（Input Metrics）
直接影響北極星的行為指標，PM 日常追蹤用。
範例：「新功能採用率」、「Day-7 留存率」

### 3. 護欄指標（Guardrail Metrics）
確保我們沒有為了提升北極星而犧牲其他東西。
範例：「客訴率 < 0.5%」、「頁面載入時間 < 2s」

### 4. 診斷指標（Diagnostic Metrics）
出問題時用來 debug，不需要每天看。
範例：「步驟 2 的 drop-off rate」

---

## 定義一個好的 Metric

每個 metric 必須包含：
- **分子**：數什麼
- **分母**：除以什麼（沒有分母的數字通常沒意義）
- **時間窗口**：日/週/月
- **追蹤方式**：從哪裡拿這個數字（GA4、BigQuery、mParticle...）

範例：
```
指標名稱：功能採用率
分子：過去 7 天內使用過 [功能X] 的 unique users
分母：過去 7 天內登入的 active users
時間窗口：滾動 7 天
追蹤方式：BigQuery event_name = 'feature_x_click'
目標：上線後第 4 週達到 30%
```

---

## Leading vs Lagging

| 類型 | 特性 | 範例 |
|------|------|------|
| Leading（領先） | 快速反饋，早期信號 | 功能點擊率、頁面停留時間 |
| Lagging（落後） | 真正的業務結果，慢 | 訂閱轉換率、月收入 |

好的 metrics 計畫要兼顧兩者：leading 指標告訴你方向對不對，lagging 指標告訴你有沒有真的到達目的地。

---

## 完成標準

一個 Metric 算「完成」需滿足以下全部條件。
**每定義完一個指標，對照一次。不滿足的項目主動提示補充，不能直接輸出。**

- [ ] 有明確的分子（數什麼，精確到 event name 或欄位）
- [ ] 有明確的分母（除以什麼，不能是「所有用戶」這種模糊說法）
- [ ] 時間窗口已指定（日/滾動 7 天/月）
- [ ] 追蹤方式已確認（BigQuery / GA4 / mParticle，最好精確到 event_name）
- [ ] 有目標值（上線後第幾週達到多少）
- [ ] 已標明指標類型（北極星 / 驅動 / 護欄 / 診斷）

**不滿足時的標準處理：**
```
⚠️ 這個 Metric 定義不完整，缺少 [項目]：
- 建議補充：[具體說明]
```

特別注意：「提升 X%」不是完整目標值，需要有 baseline 數字才能換算。
若 baseline 未知，標記「需先建立 baseline 量測，目標值待定」。

**Baseline 不存在時的標準處理：**

不是標記完就結束，要給使用者明確的下一步：

1. 確認追蹤方式（event_name 或 BigQuery 查詢語法）已確定
2. 上線前跑一週 baseline 量測，記錄現況數字
3. 根據 baseline 設定合理目標值（通常是 baseline 的 110-130%）

常見 baseline 建立方式：
- BigQuery：先寫查詢確認 event 有在被記錄，跑過去 30 天歷史數字
- GA4：確認 event 已部署，Custom Report 跑 30 天歷史
- mParticle：確認 event schema，跑 data plan 確認覆蓋率

**Baseline 不存在時的標準處理：**

不是標記完就結束，要給使用者明確的下一步：



常見 baseline 建立方式（依追蹤工具）：
- BigQuery：先寫查詢確認 event 有在被記錄，跑過去 30 天的數字
- GA4：確認 event 已部署，Custom Report 跑 30 天歷史
- mParticle：確認 event schema，跑 data plan 確認覆蓋率
