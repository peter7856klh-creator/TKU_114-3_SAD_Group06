# 循序圖追溯與訊息表 (Sequence Trace)

## I. 循序圖範圍卡

### 循序圖 1：主要流程 (SEQ-01)
* **使用案例**：建立揪團與顯示看板
* **流程類型**：主要
* **前置條件**：使用者已登入，尚未建立揪團
* **參與者**：團主、GroupJoinForm、GroupJoinController、Group
* **邊界類別**：GroupJoinForm
* **控制類別**：GroupJoinController
* **實體類別**：Group、StatusLog
* **成功或失敗結果**：建立成功並顯示可加入的卡片
* **對應後置條件**：Group 狀態為 open，卡片顯示正確

### 循序圖 2：替代或例外流程 (SEQ-02)
* **使用案例**：加入揪團時額滿或重複加入
* **流程類型**：替代／例外
* **前置條件**：使用者已看到看板，Group 已存在
* **參與者**：使用者、GroupJoinForm、GroupJoinController、Group、JoinRecord
* **邊界類別**：GroupJoinForm
* **控制類別**：GroupJoinController
* **實體類別**：Group、JoinRecord
* **成功或失敗結果**：顯示額滿或重複加入警告
* **對應後置條件**：不建立新的加入紀錄

---

## J. 循序圖訊息表

| 順序 | 發送者 | 接收者 | 訊息 | 輸入 | 回傳 | 對應類別操作 | 對應流程步驟 |
| ---: | --- | --- | --- | --- | --- | --- | --- |
| 1 | 團主 | GroupJoinForm | 提交建立揪團表單 | title、destination | | submitCreateForm() | 1 |
| 2 | GroupJoinForm | GroupJoinController | 呼叫建立流程 | 表單資料 | | createGroupFlow() | 2 |
| 3 | GroupJoinController | Group | 建立 Group 物件 | 基本資料 | | createGroup() | 3 |
| 4 | Group | StatusLog | 記錄初始狀態 | open | | appendLog() | 4 |
| 5 | GroupJoinController | GroupJoinForm | 回傳建立結果 | 成功 | 建立成功 | | 5 |
| 6 | 使用者 | GroupJoinForm | 提交加入請求 | groupId | | submitJoinForm() | 6 |
| 7 | GroupJoinController | Group | 檢查名額與狀態 | groupId | 是否可加入 | updateStatus() | 7 |
| 8 | GroupJoinController | JoinRecord | 建立加入紀錄 | userId、groupId | 建立成功 | recordJoinEvent() | 8 |

---

## K. alt 與 loop 片段檢查

| 片段 | 守衛或重複條件 | 來源規則 | 結果或終止條件 | 是否可測試 |
| --- | --- | --- | --- | --- |
| alt 分支 1 | 若人數未達上限 | FR-03 | 建立加入紀錄並顯示成功 | 是 |
| alt 分支 2 | 若已達上限 | FR-03 | 顯示額滿警告 | 是 |
| alt 其他分支 | 若重複加入 | BR-02 | 拒絕建立新紀錄 | 是 |
| loop | 重新計算剩餘名額直到流程結束 | FR-03 | 結束時顯示最新名額 | 是 |