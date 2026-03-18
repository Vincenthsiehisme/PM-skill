# User Story Guide

## 格式

```
As a [具體的使用者角色],
I want to [具體的行為/功能],
so that [明確的業務價值/目的].
```

**Acceptance Criteria 格式（Given/When/Then）：**
```
Given [前置條件]
When [使用者動作]
Then [預期結果]
```

---

## 拆解方法

一個功能通常可以拆成以下幾類 Story：

1. **Happy Path**：正常流程下的主要行為
2. **Edge Case**：邊界情況（空資料、錯誤輸入）
3. **Permission / Role**：不同角色的行為差異
4. **Error Handling**：失敗時怎麼辦

---

## 常見錯誤

❌ `As a user, I want a dashboard` — 太模糊，沒有 why  
✅ `As a content editor, I want to see today's article view count on the dashboard, so that I can adjust publishing strategy immediately`

❌ `As a user, I want the system to be fast` — 不是 story，是 NFR（非功能需求）  
✅ 把效能需求放在 Acceptance Criteria 裡：`Then the page loads within 2 seconds`

---

## Ticket 拆解原則

每個 ticket 應該：
- 可以在 1–3 天內完成
- 有獨立的 value（不是「後端 API」這種半成品）
- 有明確的 done criteria

**拆太大的訊號：** Story point > 8、需要超過一個人、無法單獨 demo

---

## 完成標準

一則 User Story 算「完成」需滿足以下全部條件。
**每寫完一則，對照一次。不滿足的項目主動提示使用者補充，不能直接跳下一則。**

- [ ] Story 本體包含 who / what / why 三個元素，缺一不完整
- [ ] 至少 1 條 AC，格式為 Given / When / Then
- [ ] Happy Path 的 AC 已涵蓋（正常流程走完的預期結果）
- [ ] 若功能涉及不同角色或權限，有對應的 Permission story
- [ ] 若功能有錯誤情境（網路失敗、空資料、權限不足），有對應的 Error Handling AC

**不滿足時的標準處理：**
```
⚠️ 這則 Story 缺少 [項目]，請補充後再繼續：
- 建議補充：[具體說明缺什麼]
```

不要假設、不要跳過、不要用 [待確認] 帶過 Story 本身的結構問題。

---

## 覆蓋度檢查（一個功能寫完幾則才夠）

寫完所有 Story 後，對照以下清單確認覆蓋度。
**不是每個功能都需要全部類型，但每個類型的「需要嗎？」都要問過。**

| Story 類型 | 需要嗎？ | 判斷方式 |
|-----------|---------|---------|
| Happy Path | ✅ 永遠需要 | 正常流程走完的主要行為，至少 1 則 |
| Edge Case | 看情況 | 有空資料、邊界值、異常輸入的情境就需要 |
| Permission / Role | 看情況 | 有超過 1 種角色看到不同內容就需要 |
| Error Handling | 幾乎總是需要 | 有網路請求、資料寫入、第三方依賴就需要 |
| Empty State | 常常被遺漏 | 第一次使用、資料為空時使用者看到什麼 |
| Non-functional（NFR） | 看情況 | 效能、無障礙、國際化需求，寫進 AC 而非獨立 Story |

**覆蓋度不足的訊號：**
- 只有 1 則 Story 就完成了整個功能 → 通常代表 edge case 沒寫
- 所有 Story 都是 Happy Path → Error Handling 和 Empty State 被跳過
- Story 數量超過 8 則 → 功能可能太大，考慮拆成兩個功能

**寫完後主動問使用者：**
「這個功能有沒有不同角色（如管理員 vs 一般用戶）看到不同內容？有沒有需要處理空資料或錯誤的情境？」
