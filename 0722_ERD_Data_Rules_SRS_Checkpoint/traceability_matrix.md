# 需求追溯矩陣 (Traceability Matrix)

## N. 追溯表

| 需求／功能 | 來源 | 業務規則 | 使用案例/模型 | 驗收條件 | 目前狀態 |
| --- | --- | --- | --- | --- | --- |
| FR-01 建立揪團 | 0714 需求文件包 | BR-01 | UC-01、Group | AC-01-01、AC-01-03 | 已納入 ERD 與資料字典 |
| FR-02 看板瀏覽 | 0714 需求文件包 | 無 | UC-02、Group | AC-02-01、AC-02-02 | 已納入 Group 與資料流 |
| FR-03 顯示剩餘名額 | 0714 需求文件包 | BR-02 | UC-03、Group | AC-05-01 | 已納入 Group.current_member_count |
| FR-04 加入揪團 | 0714 需求文件包 | BR-02 | UC-04、JoinRecord | AC-03-01、AC-03-02 | 已納入 JoinRecord |
| FR-05 分攤計算 | 0714 需求文件包 | BR-03 | UC-05、SplitResult | AC-04-01、AC-NFR-01-01 | 已納入 SplitResult |
| FR-06 滿額防呆 | 0714 需求文件包 | BR-02 | UC-06、Group.status | AC-03-02、AC-05-01 | 已納入狀態與規則 |
| FR-07 看板顯示剩餘名額 | 0714 需求文件包 | BR-02 | UC-06、Group | AC-05-01 | 已納入 Group 與資料項目 |
| FR-08 完整流程 | 0714 需求文件包 | 依整體流程 | UC-01 至 UC-06 | AC-01-01 至 AC-05-01 | 已納入整體流程與 SRS |