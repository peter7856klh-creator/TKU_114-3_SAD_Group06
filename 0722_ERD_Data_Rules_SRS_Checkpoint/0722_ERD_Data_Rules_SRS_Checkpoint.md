# 0722 工作紙

## 0722 工作表

### A. 小組基本資料

```text
課程名稱：系統分析與設計
日期：115/07/22
小組名稱：Group06
專案名稱：校園揪團與共乘／分攤平台
組員與分工：徐肇鴻（文件與整合）、小組成員（需求與實作確認）
GitHub（程式碼協作平台）儲存庫連結：https://github.com/peter7856klh-creator/TKU_114-3_SAD_Group06
0714 成果資料夾連結：https://github.com/peter7856klh-creator/TKU_114-3_SAD_Group06/tree/main/0714_User_Stories_Acceptance_Implementation
0721 成果資料夾連結：本文件所在資料夾
```

### B. 分析基準確認

| 成果 | 版本或連結 | 本次使用範圍 | 待確認問題 |
| --- | --- | --- | --- |
| 專案章程 | updated_code_agent_brief.md | 以第一個可操作切片的專案目標與保留／修正範圍為基準 | 無 |
| 功能性需求與業務規則 | functional_requirements.md、business_rules_data_constraints.md | FR-01 至 FR-08、BR-01 至 BR-03、DR-01 至 DR-03 | 候補與取消後歷史紀錄規則仍待確認 |
| 使用案例與描述 | 0721 工作紙中的 UC-01 至 UC-06 | 發起揪團、看板瀏覽、加入揪團、分攤計算與滿額防呆 | 是否納入候補與地點選單 |
| 目標流程 | 0721 工作紙中的 T-01 至 T-08 | 發起揪團 → 看板顯示 → 加入揪團 → 分攤結果 | 加入成功後是否立即重新計算看板資訊 |
| DFD 與資料字典 | 0721 工作紙中的 D1、D2、DD-01 至 DD-10 | 揪團資料、成員與分攤紀錄、加入請求與分攤基準 | 剩餘名額與分攤結果是否應保存或計算 |
| 原型與假資料 | 目前可操作切片 | 發起表單、看板卡片、加入操作、分攤結果與滿額提示 | 狀態欄位命名需與資料字典一致 |

### C. 實體候選表

| 編號 | 候選項目 | 類型 | 來源 | 判斷理由 | 保留／合併／排除 |
| --- | --- | --- | --- | --- | --- |
| EC-01 | 揪團 | 實體 | FR-01、FR-02、FR-03 | 需要獨立識別且保存基本資訊與狀態 | 保留 |
| EC-02 | 使用者 | 實體 | FR-04、FR-05 | 需要辨識加入者與其加入紀錄 | 保留 |
| EC-03 | 加入紀錄 | 關聯實體 | FR-04、BR-02 | 可保存加入時間、狀態與加入順序 | 保留，作為 M:N 拆解結果 |
| EC-04 | 分攤結果 | 實體 | FR-05、BR-03 | 可保存每次分攤基準與應付金額 | 保留 |
| EC-05 | 出發地／目的地 | 屬性 | FR-01、FR-03 | 描述揪團內容，不需要獨立識別 | 合併至揪團 |
| EC-06 | 剩餘名額 | 衍生資料 | FR-03、FR-07 | 可由人數上限與加入數計算 | 不作為主實體，改為計算欄位 |
| EC-07 | 揪團狀態 | 值或屬性 | FR-06、BR-02 | 描述揪團目前狀態，不必獨立成實體 | 合併至揪團 |
| EC-08 | 加入請求 | 資料流 | FR-04 | 是流程輸入，不是需持久保存的業務對象 | 排除，改為資料流 |

### D. 實體定義表

| 實體英文名稱 | 中文名稱 | 業務用途 | 主鍵 | 重要屬性 | 必填／選填 | 資料來源 | 對應需求／規則 | 待確認事項 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Group | 揪團 | 保存一筆揪團的基本資訊與目前狀態 | group_id | title、start_point、destination、departure_time、max_people、total_cost、current_member_count、status | 必填：title、start_point、destination、departure_time、max_people、total_cost；選填：status | FR-01、FR-02、FR-03、DR-01 | FR-01、FR-02、FR-03、BR-01、BR-02 | 是否需新增候補欄位 |
| User | 使用者 | 保存加入揪團的使用者識別與基本資訊 | user_id | name、email、role | 必填：name；選填：email、role | FR-04、FR-05 | FR-04、FR-05 | 本期是否使用正式登入 |
| JoinRecord | 加入紀錄 | 保存使用者與揪團的關聯事件，包含加入時間與狀態 | join_id | group_id、user_id、joined_at、status、sequence_no | 必填：group_id、user_id、joined_at、status；選填：sequence_no | FR-04、BR-02、DR-02 | FR-04、BR-02 | 取消後是否保留歷史紀錄 |
| SplitResult | 分攤結果 | 保存每次分攤計算結果與分攤基準 | split_id | group_id、join_id、share_amount、base_rule | 必填：group_id、join_id、share_amount、base_rule | FR-05、BR-03、AC-04-01 | FR-05、BR-03 | 是否需保存多次計算版本 |
| StatusLog | 狀態記錄 | 保存揪團相關狀態變更歷程 | status_log_id | group_id、old_status、new_status、changed_at | 必填：group_id、old_status、new_status、changed_at | FR-06、FR-07 | FR-06、FR-07 | 是否僅記錄最終狀態即可 |

### E. 屬性定義表

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

### F. 關係、基數與選擇性

| 編號 | 兩端實體 | 關係動詞 | 雙向業務敘述 | 基數 | 選擇性 | 支援規則 | 外來鍵或待確認結構 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| REL-01 | Group ↔ JoinRecord | 擁有 / 屬於 | 一個揪團可以有多筆加入紀錄；每筆加入紀錄必須屬於一個揪團 | 1:M | 0..* / 1..1 | BR-02 | JoinRecord.group_id FK |
| REL-02 | User ↔ JoinRecord | 產生 / 屬於 | 一位使用者可以有多筆加入紀錄；每筆加入紀錄必須屬於一位使用者 | 1:M | 0..* / 1..1 | FR-04 | JoinRecord.user_id FK |
| REL-03 | Group ↔ SplitResult | 產生 / 屬於 | 一個揪團可以產生多筆分攤結果；每筆分攤結果必須屬於一個揪團 | 1:M | 0..* / 1..1 | FR-05 | SplitResult.group_id FK |
| REL-04 | JoinRecord ↔ SplitResult | 生成 / 對應 | 每筆加入紀錄可對應 0 或 1 筆分攤結果；每筆分攤結果對應一筆加入紀錄 | 1:0..1 | 0..1 / 1..1 | FR-05 | SplitResult.join_id FK |
| REL-05 | Group ↔ StatusLog | 變更 / 記錄 | 一個揪團可以有多筆狀態記錄；每筆狀態記錄必須屬於一個揪團 | 1:M | 0..* / 1..1 | FR-06、FR-07 | StatusLog.group_id FK |

### G. M:N 拆解紀錄

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
```

### H. 基礎正規化與資料問題

| 資料問題編號 | 問題類型 | 症狀 | 可能原因 | 影響 | 修正方式 | 需要同步更新的文件 |
| --- | --- | --- | --- | --- | --- | --- |
| DATA-01 | 重複群組 | 以逗號分隔方式保存多位加入者 | 未拆解多對多關係 | 無法單獨查詢與驗證每位加入者 | 建立 JoinRecord 實體 | ERD、資料字典、原型假資料 |
| DATA-02 | 衍生資料未定義 | 原型中同時出現 remainingSeats 與 current_member_count | 未明確標示維護來源 | 可能發生資料不一致 | 以 current_member_count 為來源，remainingSeats 由計算得出 | 資料字典、原型、驗收條件 |
| DATA-03 | 屬性放錯實體 | 分攤金額與分攤基準直接寫在揪團資料中 | 未建立分攤結果實體 | 難以保存多次計算結果與歷史版本 | 建立 SplitResult 實體 | ERD、DFD、SRS v1 |
| DATA-04 | 狀態值不一致 | 實作品中 status 與 fullStatus 同時出現 | 名稱與規則未統一 | 會造成流程與驗收判斷歧義 | 以 status 與 full 狀態規則統一於資料字典 | 原型、資料字典、測試 |

### I. ERD 自我檢核

| 檢查項目 | 結果 | 備註 |
| --- | --- | --- |
| 至少 5 個實體 | 是 | Group、User、JoinRecord、SplitResult、StatusLog |
| 每個實體有主鍵 | 是 | 每個實體皆有識別碼 |
| 每個實體有屬性與來源 | 是 | 已對應需求與資料字典 |
| 至少 5 組關係 | 是 | REL-01 至 REL-05 |
| 至少 1 個 M:N 已拆解 | 是 | Group 與 User 已拆成 JoinRecord |
| 有關聯實體 | 是 | JoinRecord |
| 關係有雙向業務敘述 | 是 | 已寫成可驗證規則 |
| 每端有基數與選擇性 | 是 | 已標示 |
| 必要位置有外來鍵 | 是 | JoinRecord 與 SplitResult 已標示 |
| 沒有把值誤畫成實體 | 是 | remainingSeats、status 等已回到屬性或計算值 |
| 沒有無規則支持的關係 | 是 | 每條關係可對應業務規則 |
| 圖檔與原始檔可開啟 | 待補 | 建議更新至 GitHub 儲存庫 |

### J. DFD／ERD／原型假資料一致性矩陣

| 資料概念 | DFD 中位置 | ERD 中位置 | 原型假資料/畫面 | 結果 |
| --- | --- | --- | --- | --- |
| 揪團資料 | D1 揪團資料 | Group | activities[] / group cards | 一致 |
| 加入請求 | 加入請求資料流 | JoinRecord 入口資料 | joinRequest | 一致 |
| 成員與分攤紀錄 | D2 成員與分攤紀錄 | JoinRecord、SplitResult | members[] / splitAmount | 一致 |
| 剩餘名額 | D1 揪團資料 | Group.current_member_count / 計算值 | remainingSeats | 部分一致，建議改為計算值 |
| 分攤結果 | 分攤計算結果資料流 | SplitResult | splitAmount | 一致 |
| 揪團狀態 | D1 揪團資料 | Group.status | status | 一致 |
| 加入順序 | D2 成員與分攤紀錄 | JoinRecord.sequence_no | join order | 待補 |
| 狀態歷程 | D2 或 D1 相關資料 | StatusLog | 目前原型未顯示 | 待補 |

### K. SRS v1 完整性檢核

| 章節 | 狀態 | 備註 |
| --- | --- | --- |
| 文件資訊 | 已完成 | 版本、日期、小組與成果連結已整理 |
| 簡介 | 已完成 | 專案背景與本次範圍已說明 |
| 背景與問題定義 | 已完成 | 與 0714 需求包一致 |
| 系統範圍 | 已完成 | 範圍內／範圍外已列出 |
| 利害關係人與角色 | 已完成 | 團主、團員、一般使用者已納入 |
| 功能性需求 | 已完成 | FR-01 至 FR-08 已整理 |
| 非功能性需求 | 已完成 | 以現有 NFR 文件為基準 |
| 業務規則 | 已完成 | BR-01 至 BR-03 已整理 |
| 資料需求 | 已完成 | DR-01 至 DR-03 已整理 |
| 使用案例與分析模型 | 已完成 | 以 0721 DFD 與 ERD 為基準 |
| 原型與畫面對照 | 已完成 | 已對照主要欄位與資料字典 |
| 驗收條件 | 已完成 | AC-01-01 至 AC-05-01 已整理 |
| 可追溯性 | 已完成 | 已建立需求至模型與測試的連結 |
| 待確認問題 | 已完成 | 三筆主要待確認點已整理 |
| 版本與變更紀錄 | 已完成 | 以 v1.0 與本次成果為基準 |

### L. 基準版本紀錄

| 項目 | 內容 |
| --- | --- |
| 基準編號 | BL-SRS-01 |
| SRS 版本 | v1.0 |
| 建立日期 | 115/07/22 |
| 對應 Git 提交識別碼 | 待補上本次提交識別碼 |
| 已納入成果 | FR、NFR、BR、DFD、ERD、原型對照、需求追溯 |
| 尚未納入成果 | 候補與取消歷史規則、ERD 圖檔原始檔上傳 |
| 已知限制 | 假資料與前端儲存方式、未串接正式資料庫 |
| 待確認問題連結 | OI-01 至 OI-03 |
| 後續變更方式 | 依需求與驗收條件更新追溯表與資料模型 |

### M. 待確認問題

| 編號 | 問題內容 | 發現位置 | 影響 | 建議處理方式 | 狀態 |
| --- | --- | --- | --- | --- | --- |
| OI-01 | 取消加入後是否保留完整歷史紀錄 | JoinRecord、原型流程 | 影響資料保存與畫面顯示 | 先保留 cancelled 狀態與歷史紀錄，待確認業務政策 | 待確認 |
| OI-02 | 候補功能是否在本期範圍內 | FR-07、流程與原型 | 影響候補實體與流程 | 本期先不實作候補，列為後續待辦 | 待確認 |
| OI-03 | 分攤結果是否需保存多次重新計算版本 | SplitResult、驗收條件 | 影響是否需要歷史版本紀錄 | 先以單次現況結果保存，若需求增加再擴充 | 待確認 |

### N. 追溯表

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

### O. 程式碼代理使用紀錄

| 使用工具 | 任務目標 | 輸入文件版本 | 提示詞連結 | 工具建議摘要 | 採用內容與理由 | 未採用內容與理由 | 人工修正 | 檢查結果 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Code Agent | 盤點原型欄位與 ERD 差異 | 0714 需求包、0721 DFD、資料字典 | 0721 工作紙中所整理之提示詞 | 建議統一 status 與 remainingSeats 命名，並建立 JoinRecord | 採用 JoinRecord 與統一狀態命名 | 未採用新增新實體與新功能 | 以人工判斷保留目前範圍 | 已納入資料字典與 ERD |

### P. 期中影片建議內容清單

| 項目 | 建議呈現內容 | 可對應成果 |
| --- | --- | --- |
| 問題與目標使用者 | 揪團發起、加入與分攤的使用情境 | 0714 需求與使用案例 |
| 系統範圍 | 發起、看板、加入、分攤與滿額防呆 | FR-01 至 FR-08 |
| 核心需求與規則 | 人數上限、滿額防呆、分攤基準 | BR-01 至 BR-03 |
| 使用案例與流程 | 發起揪團 → 看板 → 加入 → 分攤 | 0721 工作紙中的流程圖 |
| DFD 重點 | D1 揪團資料與 D2 成員與分攤紀錄 | 0721 工作紙 |
| ERD 重點 | Group、JoinRecord、SplitResult 的核心關係 | 本工作紙 |
| 原型或實作成果 | 發起表單、看板卡片、加入操作與分攤結果 | 目前可操作切片 |
| 完整追溯案例 | FR-04 至 JoinRecord 至驗收條件 | 本工作紙中的追溯表 |
| 問題與修正案例 | remainingSeats 與 status 命名差異 | 資料問題與修正表 |
| 驗收或測試證據 | 主要驗收條件與手動檢查結果 | acceptance_criteria.md |

