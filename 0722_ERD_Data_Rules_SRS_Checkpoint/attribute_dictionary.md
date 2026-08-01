# 屬性字典 (Attribute Dictionary)

## E. 屬性定義表

| 實體 | 屬性 | 中文名稱 | 類型 | PK/FK/一般 | 必填 | 格式或允許值 | 資料來源 | 對應需求／規則 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Group | group_id | 揪團編號 | 文字 | PK | 是 | 系統內唯一 | FR-01 | FR-01 |
| Group | title | 揪團標題 | 文字 | 一般 | 是 | 不可空白 | FR-01 | FR-01 |
| Group | start_point | 出發地 | 文字 | 一般 | 是 | 不可空白 | FR-01 | FR-01 |
| Group | destination | 目的地 | 文字 | 一般 | 是 | 不可空白 | FR-01 | FR-01 |
| Group | departure_time | 出發時間 | 日期時間 | 一般 | 是 | 可解析日期時間 | FR-01 | FR-01 |
| Group | max_people | 人數上限 | 整數 | 一般 | 是 | 大於 0 | BR-01 | FR-01、BR-01 |
| Group | total_cost | 總車資 | 數值 | 一般 | 是 | 大於 0 | FR-05 | FR-05 |
| Group | current_member_count | 目前加入人數 | 整數 | 一般 | 是 | 0 至 max_people | FR-03、FR-07 | FR-03、FR-07 |
| Group | status | 揪團狀態 | 文字 | 一般 | 是 | open、full、closed | FR-06 | FR-06 |
| User | user_id | 使用者編號 | 文字 | PK | 是 | 系統內唯一 | FR-04 | FR-04 |
| User | name | 使用者名稱 | 文字 | 一般 | 是 | 不可空白 | FR-04 | FR-04 |
| JoinRecord | join_id | 加入紀錄編號 | 文字 | PK | 是 | 系統內唯一 | FR-04 | FR-04 |
| JoinRecord | group_id | 揪團編號 | 文字 | FK | 是 | 需參照 Group.group_id | FR-04 | FR-04 |
| JoinRecord | user_id | 使用者編號 | 文字 | FK | 是 | 需參照 User.user_id | FR-04 | FR-04 |
| JoinRecord | joined_at | 加入時間 | 日期時間 | 一般 | 是 | 可解析日期時間 | FR-04 | FR-04 |
| JoinRecord | status | 加入狀態 | 文字 | 一般 | 是 | pending、accepted、rejected | FR-04 | FR-04 |
| JoinRecord | sequence_no | 加入順序 | 整數 | 一般 | 否 | 同一揪團內不可重複 | DR-02 | DR-02 |
| SplitResult | split_id | 分攤結果編號 | 文字 | PK | 是 | 系統內唯一 | FR-05 | FR-05 |
| SplitResult | share_amount | 每人應付金額 | 數值 | 一般 | 是 | 保留兩位小數 | BR-03 | BR-03 |
| SplitResult | base_rule | 分攤基準 | 文字 | 一般 | 是 | 總車資／加入人數 | BR-03 | BR-03 |