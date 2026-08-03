# 軟體設計文件 (Software Design Document - SDD v1)

## T. SDD v1 設計決策

| 決策編號 | 背景 | 選項 | 決定 | 理由 | 影響 | 來源 | 重查條件 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| DD-01 | 狀態轉移與畫面同步問題 | A. 前端定時 Polling<br>B. 控制器觸發 Event 同步 | 採用選項 B (控制器觸發) | 降低 Server 負擔並確保 MVC/BCE 責任分離 | GroupJoinController 負擔增加，但畫面資料嚴格一致 | PI-01 檢討 | 使用者人數突破 10,000 人需要 WebSocket 時 |
| DD-02 | 重複報名防呆機制 | A. 僅 DB 設 Unique Constraint<br>B. 控制層先檢核並丟出錯誤 | 採用選項 B | 可提供更友善的 UI 回饋 (ST-04)，而非直接報 500 錯 | 須維護 JoinRecord 檢驗邏輯 | BR-02 | 多裝置同時搶單極限情境 |
| DD-03 | 費用分攤計算時機 | A. 每次加入即時重算<br>B. 進入費用頁時呼叫 SplitCalculator | 採用選項 B | 避免頻繁發起無謂的計算，分攤資訊屬於 UI-06 獨立責任 | UI-06 開啟時需呼叫計算 API | FR-05 | 需支援即時線上拆帳推播時 |
