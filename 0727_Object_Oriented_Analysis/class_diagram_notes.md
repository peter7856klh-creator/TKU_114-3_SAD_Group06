# 類別圖完成檢查表 (Class Diagram Notes)

## H. 類別圖完成檢查

| 檢查項目 | 是／否 | 證據或待修正 |
| --- | --- | --- |
| 至少 6 個核心類別 | 是 | Group、User、JoinRecord、SplitResult、StatusLog、GroupJoinController |
| 每個核心類別有責任 | 是 | 已在責任卡與明細表中說明 |
| 屬性支援責任 | 是 | 例如 Group 的 maxPeople 與 status 支援狀態與名額 |
| 操作對應使用案例 | 是 | createGroup()、joinGroupFlow()、calculateSplit() |
| 至少 5 條關係 | 是 | 已列 5 條關聯 |
| 核心關係有多重性 | 是 | 已標示 1 與 0..* |
| 關係類型有業務語意 | 是 | 關聯、歷史與分攤關係皆有理由 |
| 可辨識邊界、控制、實體 | 是 | GroupJoinForm、GroupJoinController、Group/JoinRecord |
| 與實體關係圖相容 | 是 | 與 ERD 的主鍵與關聯一致 |
| 沒有把程式檔直接當類別 | 是 | 未把 UI 檔案或資料表直接視為分析類別 |