# 模型－畫面－資料－驗收一致性矩陣 (Model-Screen-Data Acceptance Matrix)

## K. 模型－畫面－資料－驗收一致性矩陣

| 編號 | 需求／規則 | 使用案例／模型 | 類別責任 | 畫面／狀態 | 資料 | 驗收／測試 | 判斷 | 待處理 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| TR-01 | FR-01 發起揪團 | UC-01 / SEQ-01 | Group: createGroup() | UI-03 (正常) | Group fields (title, maxPeople) | 能夠建立 open 狀態的揪團卡片 | 一致 | 無 |
| TR-02 | FR-03 名額限制 | UC-03 / State: full | Group: calculateRemainingSeats() | UI-02 (ST-03 停用) | maxPeople, currentMemberCount | 人數滿時按鈕自動變額滿且不可按 | 一致 | 無 |
| TR-03 | BR-02 重複加入防呆 | UC-03 / SEQ-02 | GroupJoinController: joinGroupFlow() | UI-05 (ST-04 錯誤) | JoinRecord (groupId, userId) | 同一 User 不可重複建立 JoinRecord | 一致 | 無 |
| TR-04 | FR-05 費用分攤 | UC-04 / SEQ-03 | SplitCalculator: calculateShare() | UI-06 (正常) | SplitResult: shareAmount | 應付金額 = 總金額 / 目前有效加入人數 | 一致 | 無 |
| TR-05 | FR-06 關閉揪團 | UC-06 / State: closed | Group: updateStatus() | UI-08 (正常) | Group.status = 'closed' | 關閉後不可再加入或取消 | 一致 | 無 |
| TR-06 | BR-02 取消報名 | UC-05 / SEQ-04 | JoinRecord: updateStatus() | UI-04 (正常) | JoinRecord.status = 'cancelled' | 取消後名額釋出，Group status 變回 open | 部分一致 | 需補齊 StatusLog 同步 |
| TR-07 | 權限管控 | UC-06 / 系統規則 | GroupJoinController | UI-08 (ST-05 權限不足) | User.role | 非團主進入跳轉並提示權限錯誤 | 一致 | 無 |
| TR-08 | 歷史追蹤 | FR-07 / StatusLog | StatusLog: appendLog() | UI-04 (正常) | StatusLog (oldStatus, newStatus) | 每次狀態轉移均會產生一筆 Log | 一致 | 無 |
