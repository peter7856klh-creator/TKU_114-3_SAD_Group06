# 模型驅動實作任務書 (Model-Driven Code Agent Brief)

## Q. 更新後的模型驅動實作任務書

```text
任務名稱：補齊加入揪團與滿額狀態轉移的責任分配
需求與模型版本：FR-03、FR-04、UC-03、Group/JoinRecord/StatusLog v1
目前症狀：加入後畫面顯示名額與 Group.status 可能不同步
重現方式：建立一筆揪團後連續加入至滿額，觀察看板與狀態顯示
正確責任分配：Group 負責名額與狀態，JoinRecord 負責加入紀錄，GroupJoinController 協調流程
涉及類別：Group、JoinRecord、GroupJoinController
涉及訊息與互動順序：提交加入請求 → 檢查名額 → 建立紀錄 → 更新狀態 → 更新畫面
合法狀態轉移：open → full、full → open、full → closed
非法狀態轉移：closed → open、full → 任意狀態值
要保留：目前建立揪團與加入流程
要修改：狀態更新與畫面同步邏輯
要新增：狀態歷程記錄
要刪除：畫面直接改動狀態的邏輯
本次不做：候補與正式登入
不可自行判斷：取消後是否保留完整歷史
主要流程測試：建立揪團、加入、額滿
替代或例外測試：重複加入、額滿後再加入
狀態轉移測試：open/full/closed 轉換
完成定義：名額與狀態可同步，測試通過
完成後回報格式：修改檔案、責任分配、測試結果與證據