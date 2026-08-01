# 關係與業務規則 (Relationship & Business Rules)

## F. 關係、基數與選擇性

| 編號 | 兩端實體 | 關係動詞 | 雙向業務敘述 | 基數 | 選擇性 | 支援規則 | 外來鍵或待確認結構 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| REL-01 | Group ↔ JoinRecord | 擁有 / 屬於 | 一個揪團可以有多筆加入紀錄；每筆加入紀錄必須屬於一個揪團 | 1:M | 0..* / 1..1 | BR-02 | JoinRecord.group_id FK |
| REL-02 | User ↔ JoinRecord | 產生 / 屬於 | 一位使用者可以有多筆加入紀錄；每筆加入紀錄必須屬於一位使用者 | 1:M | 0..* / 1..1 | FR-04 | JoinRecord.user_id FK |
| REL-03 | Group ↔ SplitResult | 產生 / 屬於 | 一個揪團可以產生多筆分攤結果；每筆分攤結果必須屬於一個揪團 | 1:M | 0..* / 1..1 | FR-05 | SplitResult.group_id FK |
| REL-04 | JoinRecord ↔ SplitResult | 生成 / 對應 | 每筆加入紀錄可對應 0 或 1 筆分攤結果；每筆分攤結果對應一筆加入紀錄 | 1:0..1 | 0..1 / 1..1 | FR-05 | SplitResult.join_id FK |
| REL-05 | Group ↔ StatusLog | 變更 / 記錄 | 一個揪團可以有多筆狀態記錄；每筆狀態記錄必須屬於一個揪團 | 1:M | 0..* / 1..1 | FR-06、FR-07 | StatusLog.group_id FK |

---

## G. M:N 拆解紀錄

```text
原始多對多關係：Group 與 User
為什麼是 M:N：一個使用者可以加入多個揪團；一個揪團可以有多位使用者加入。

直接保留會遺失的資料或規則：
1. 加入時間無法單獨保存。
2. 加入狀態與加入順序無法保存。
3. 同一使用者重複加入同一揪團的情況難以約束。

關聯實體英文名稱：JoinRecord
關聯實體中文名稱：加入紀錄
主鍵：join_id
外來鍵 A：group_id
外來鍵 B：user_id
關聯本身的屬性：joined_at、status、sequence_no

拆解後關係 1：Group 1:M JoinRecord
拆解後關係 2：User 1:M JoinRecord

唯一限制：UNIQUE(group_id, user_id, status)
支持業務規則：同一使用者對同一揪團不得重複建立有效加入紀錄。
待確認問題：取消後是否保留歷史紀錄或以新紀錄替代。