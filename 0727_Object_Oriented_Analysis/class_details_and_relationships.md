# 類別明細與關係 (Class Details and Relationships)

## E. 類別明細

| 類別 | 類型 | 責任摘要 | 重要屬性 | 主要操作 | 來源 |
| --- | --- | --- | --- | --- | --- |
| Group | Entity | 保存揪團資料與狀態 | groupId、status、maxPeople | createGroup()、updateStatus() | FR-01、FR-03 |
| User | Entity | 代表參與者 | userId、name | joinGroup() | FR-04 |
| JoinRecord | Entity | 記錄加入歷史 | joinId、joinedAt、status | recordJoinEvent() | FR-04、BR-02 |
| SplitResult | Entity | 保存分攤結果 | splitId、shareAmount | calculateSplit() | FR-05 |
| GroupJoinController | Control | 協調流程與規則 | currentAction、resultMessage | joinGroupFlow() | UC-03 |
| GroupJoinForm | Boundary | 接收與顯示操作 | formData、message | submitCreateForm() | 原型 |

---

## F. 類別關係與多重性

| 類別 A | 關係 | 類別 B | A 端多重性 | B 端多重性 | 業務規則或模型依據 |
| --- | --- | --- | --- | --- | --- |
| Group | 擁有 | JoinRecord | 1 | 0..* | 一個揪團可有多筆加入紀錄 |
| User | 參與 | JoinRecord | 1 | 0..* | 一位使用者可有多筆加入紀錄 |
| Group | 產生 | SplitResult | 1 | 0..* | 一個揪團可產生多筆分攤結果 |
| JoinRecord | 對應 | SplitResult | 0..1 | 1 | 每次加入可對應一筆分攤結果 |
| Group | 記錄 | StatusLog | 1 | 0..* | 一個揪團可有多筆狀態歷程 |

### 關係判斷與決策紀錄
* **這是一般關聯、聚合、組合或一般化**：一般關聯
* **整體移除時，部分是否失去存在意義**：是，JoinRecord 仍需保留歷史
* **部分是否可同時屬於多個整體**：否，每筆 JoinRecord 只屬於一個 Group
* **子類別是否真的是父類別的一種**：否
* **最後決定**：採用一般關聯與關聯類別
* **理由**：加入紀錄與狀態記錄與揪團之間是事件與歷史關係，不適合用一般化