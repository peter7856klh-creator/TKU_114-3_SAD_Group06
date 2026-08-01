# 實體定義表 (Entity Definitions)

## D. 實體定義表

| 實體英文名稱 | 中文名稱 | 業務用途 | 主鍵 | 重要屬性 | 必填／選填 | 資料來源 | 對應需求／規則 | 待確認事項 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Group | 揪團 | 保存一筆揪團的基本資訊與目前狀態 | group_id | title、start_point、destination、departure_time、max_people、total_cost、current_member_count、status | 必填：title、start_point、destination、departure_time、max_people、total_cost；選填：status | FR-01、FR-02、FR-03、DR-01 | FR-01、FR-02、FR-03、BR-01、BR-02 | 是否需新增候補欄位 |
| User | 使用者 | 保存加入揪團的使用者識別與基本資訊 | user_id | name、email、role | 必填：name；選填：email、role | FR-04、FR-05 | FR-04、FR-05 | 本期是否使用正式登入 |
| JoinRecord | 加入紀錄 | 保存使用者與揪團的關聯事件，包含加入時間與狀態 | join_id | group_id、user_id、joined_at、status、sequence_no | 必填：group_id、user_id、joined_at、status；選填：sequence_no | FR-04、BR-02、DR-02 | FR-04、BR-02 | 取消後是否保留歷史紀錄 |
| SplitResult | 分攤結果 | 保存每次分攤計算結果與分攤基準 | split_id | group_id、join_id、share_amount、base_rule | 必填：group_id、join_id、share_amount、base_rule | FR-05、BR-03、AC-04-01 | FR-05、BR-03 | 是否需保存多次計算版本 |
| StatusLog | 狀態記錄 | 保存揪團相關狀態變更歷程 | status_log_id | group_id、old_status、new_status、changed_at | 必填：group_id、old_status、new_status、changed_at | FR-06、FR-07 | FR-06、FR-07 | 是否僅記錄最終狀態即可 |