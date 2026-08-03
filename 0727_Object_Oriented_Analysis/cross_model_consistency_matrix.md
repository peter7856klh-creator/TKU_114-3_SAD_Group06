# 跨模型一致性矩陣 (Cross Model Consistency Matrix)

## O. 跨模型一致性矩陣

| 情境或規則 | 需求 | 使用案例 | ERD（實體關係圖） | Class（類別圖） | Sequence（循序圖） | State（狀態圖） | 實作 | 測試 | 結論 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 建立揪團 | FR-01 | UC-01 | 有 Group 實體 | Group、GroupJoinController | SEQ-01 | open | 已有表單與建立流程 | TC-01 | 一致 |
| 加入揪團 | FR-04 | UC-03 | 有 JoinRecord 實體 | JoinRecord、Group | SEQ-02 | open/full | 已建立加入紀錄 | TC-02 | 一致 |
| 滿額防呆 | FR-03 | UC-03 | 有 maxPeople 與 status | Group | SEQ-02 | full | 需確保畫面與資料同步 | TC-03 | 部分一致 |
| 分攤計算 | FR-05 | UC-04 | 有 SplitResult 實體 | SplitResult、SplitCalculator | SEQ-03 | 無 | 已有計算邏輯但未完全整合 | TC-04 | 部分一致 |
| 取消加入 | FR-04、BR-02 | UC-05 | 有 JoinRecord 狀態 | JoinRecord | SEQ-04 | open/closed | 待補完整處理 | TC-05 | 待確認 |

---

## P. 一致性問題紀錄

| 問題編號 | 症狀 | 模型依據 | 最可能原因 | 修正位置 | 驗證方式 | 狀態 |
| --- | --- | --- | --- | --- | --- | --- |
| PI-01 | 目前畫面顯示名額與 Group.status 可能不同步 | FR-03、SEQ-02 | 狀態更新與資料顯示分離 | Group、GroupJoinController | 回歸測試與介面驗證 | 待修正 |
| PI-02 | 取消加入後仍無完整歷史紀錄 | BR-02、JoinRecord | 歷史保存與取消狀態尚未完整定義 | JoinRecord、StatusLog | 狀態轉移測試 | 待確認 |
| PI-03 | 分攤結果未與最新加入人數同步 | FR-05、SplitResult | 計算邏輯與資料更新時機未整合 | SplitCalculator、SplitResult | 測試與手動驗證 | 待修正 |