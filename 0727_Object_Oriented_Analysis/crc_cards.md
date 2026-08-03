# CRC 類別責任卡 (CRC Cards)

## C. 候選類別擷取表

| 候選項目 | 來源編號 | 可能責任 | 保留／合併／移除 | 判斷理由 |
| --- | --- | --- | --- | --- |
| Group | FR-01、FR-02、FR-03 | 建立揪團、維護狀態、計算剩餘名額 | 保留 | 代表一筆揪團，且是核心業務物件 |
| User | FR-04、FR-05 | 代表發起人與加入者 | 保留 | 參與加入與分攤流程 |
| JoinRecord | FR-04、BR-02 | 記錄加入事件、順序與狀態 | 保留 | 需處理多對多關係與加入歷史 |
| SplitResult | FR-05、BR-03 | 計算分攤、顯示每人應付 | 保留 | 對應分攤需求與計算規則 |
| StatusLog | FR-06、FR-07 | 記錄狀態變更與追蹤 | 保留 | 支援滿額與關閉狀態變更 |
| ActivityBoard | 原型畫面 | 顯示卡片、顯示剩餘名額 | 合併 | 更像介面層，而非核心業務類別 |
| RemainingSeat | FR-03、FR-07 | 計算剩餘名額 | 移除為單獨類別 | 可由 Group 的方法輸出 |
| JoinRequest | FR-04 | 接收加入需求 | 合併 | 是操作事件，不需獨立存成主類別 |

---

## D. 類別責任卡

### Group (Entity)
* **主要責任 1**：管理揪團的基本資料與目前狀態
* **主要責任 2**：判斷是否達成滿額與更新剩餘名額
* **重要屬性**：groupId、title、startPoint、destination、departureTime、maxPeople、status
* **主要操作**：createGroup()、updateStatus()、calculateRemainingSeats()
* **協作者**：User、JoinRecord、StatusLog
* **來源編號**：FR-01、FR-02、FR-03
* **保留理由**：具明確責任且是需求與流程的核心物件
* **待確認問題**：候補與取消後流程是否保留歷史

### User (Entity)
* **主要責任 1**：表示發起人與加入者
* **主要責任 2**：觸發加入行為並建立加入紀錄
* **重要屬性**：userId、name、email、role
* **主要操作**：joinGroup()
* **協作者**：Group、JoinRecord
* **來源編號**：FR-04、FR-05
* **保留理由**：與加入流程有直接關聯
* **待確認問題**：是否需正式登入

### JoinRecord (Entity)
* **主要責任 1**：記錄某使用者對某揪團的加入事件
* **主要責任 2**：保存加入順序與加入狀態
* **重要屬性**：joinId、groupId、userId、joinedAt、status、sequenceNo
* **主要操作**：recordJoinEvent()
* **協作者**：Group、User、SplitResult
* **來源編號**：FR-04、BR-02
* **保留理由**：用來拆解多對多關係與保留歷史
* **待確認問題**：取消後是否保留紀錄

### SplitResult (Entity)
* **主要責任 1**：根據分攤規則計算每人應付
* **主要責任 2**：保存一次分攤結果
* **重要屬性**：splitId、groupId、joinId、shareAmount、baseRule
* **主要操作**：calculateSplit()
* **協作者**：JoinRecord、Group
* **來源編號**：FR-05、BR-03
* **保留理由**：對應分攤與金額顯示需求
* **待確認問題**：是否保留多次重算版本

### StatusLog (Entity)
* **主要責任 1**：記錄狀態變更歷程
* **主要責任 2**：支援回溯與後續追蹤
* **重要屬性**：statusLogId、groupId、oldStatus、newStatus、changedAt
* **主要操作**：appendLog()
* **協作者**：Group
* **來源編號**：FR-06、FR-07
* **保留理由**：狀態轉移必須可追蹤
* **待確認問題**：是否只記錄最終狀態

### GroupJoinController (Control)
* **主要責任 1**：協調建立揪團與加入流程
* **主要責任 2**：驗證流程條件與回傳結果
* **重要屬性**：currentAction、resultMessage
* **主要操作**：createGroupFlow()、joinGroupFlow()
* **協作者**：Group、JoinRecord、StatusLog
* **來源編號**：UC-01、UC-03
* **保留理由**：可避免畫面直接承擔業務規則
* **待確認問題**：是否再拆成更細的服務類別

### GroupJoinForm (Boundary)
* **主要責任 1**：接收發起或加入操作
* **主要責任 2**：顯示結果與錯誤訊息
* **重要屬性**：formData、message
* **主要操作**：submitCreateForm()、submitJoinForm()
* **協作者**：GroupJoinController
* **來源編號**：原型畫面
* **保留理由**：負責使用者互動與輸入輸出
* **待確認問題**：是否與 UI 元件耦合過深

### SplitCalculator (Control)
* **主要責任 1**：依據分攤規則計算每人金額
* **主要責任 2**：防止分攤邏輯散落於畫面裡
* **重要屬性**：totalAmount、memberCount
* **主要操作**：calculateShare()
* **協作者**：SplitResult、JoinRecord
* **來源編號**：FR-05、BR-03
* **保留理由**：可把分攤計算與展示分開
* **待確認問題**：是否需要保留多版本結果