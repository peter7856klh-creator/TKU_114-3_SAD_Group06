# 系統分析與設計：7/27 工作紙

### A. 小組基本資料

| 項目 | 內容 |
| --- | --- |
| 小組名稱 | Group06 |
| 專案名稱 | 校園揪團與共乘／分攤平台 |
| GitHub（程式碼協作平台）儲存庫 | https://github.com/peter7856klh-creator/TKU_114-3_SAD_Group06 |
| 今日使用分支（Branch） | main |
| 記錄日期 | 115/07/27 |
| 參與成員 | 徐肇鴻、組員 A、組員 B |

### B. 模型基準確認

| 成果 | 版本或提交識別 | 日期 | 是否採用 | 備註 |
| --- | --- | --- | --- | --- |
| 需求文件包 | 0714_需求文件包 | 115/07/27 | 是 | 以 FR、BR、DR 為主要依據 |
| 軟體需求規格書 | SRS v1 | 115/07/27 | 是 | 以功能與資料需求為基準 |
| 使用案例 | UC-01 至 UC-06 | 115/07/27 | 是 | 包含發起揪團、瀏覽看板、加入揪團、分攤計算 |
| 資料流程圖 | DFD v1 | 115/07/27 | 是 | 用於確認資料流與資料來源 |
| 實體關係圖 | ERD v1 | 115/07/27 | 是 | 用於確認主鍵、外來鍵與關聯 |
| 目前實作 | 第一個可操作切片 | 115/07/27 | 是 | 以假資料與前端互動為基礎 |

### C. 候選類別擷取表

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

### D. 類別責任卡

| 欄位 | 內容 |
| --- | --- |
| 類別名稱 | Group |
| 類型 | Entity |
| 主要責任 1 | 管理揪團的基本資料與目前狀態 |
| 主要責任 2 | 判斷是否達成滿額與更新剩餘名額 |
| 重要屬性 | groupId、title、startPoint、destination、departureTime、maxPeople、status |
| 主要操作 | createGroup()、updateStatus()、calculateRemainingSeats() |
| 協作者 | User、JoinRecord、StatusLog |
| 來源編號 | FR-01、FR-02、FR-03 |
| 保留理由 | 具明確責任且是需求與流程的核心物件 |
| 待確認問題 | 候補與取消後流程是否保留歷史 |

| 欄位 | 內容 |
| --- | --- |
| 類別名稱 | User |
| 類型 | Entity |
| 主要責任 1 | 表示發起人與加入者 |
| 主要責任 2 | 觸發加入行為並建立加入紀錄 |
| 重要屬性 | userId、name、email、role |
| 主要操作 | joinGroup() |
| 協作者 | Group、JoinRecord |
| 來源編號 | FR-04、FR-05 |
| 保留理由 | 與加入流程有直接關聯 |
| 待確認問題 | 是否需正式登入 |

| 欄位 | 內容 |
| --- | --- |
| 類別名稱 | JoinRecord |
| 類型 | Entity |
| 主要責任 1 | 記錄某使用者對某揪團的加入事件 |
| 主要責任 2 | 保存加入順序與加入狀態 |
| 重要屬性 | joinId、groupId、userId、joinedAt、status、sequenceNo |
| 主要操作 | recordJoinEvent() |
| 協作者 | Group、User、SplitResult |
| 來源編號 | FR-04、BR-02 |
| 保留理由 | 用來拆解多對多關係與保留歷史 |
| 待確認問題 | 取消後是否保留紀錄 |

| 欄位 | 內容 |
| --- | --- |
| 類別名稱 | SplitResult |
| 類型 | Entity |
| 主要責任 1 | 根據分攤規則計算每人應付 |
| 主要責任 2 | 保存一次分攤結果 |
| 重要屬性 | splitId、groupId、joinId、shareAmount、baseRule |
| 主要操作 | calculateSplit() |
| 協作者 | JoinRecord、Group |
| 來源編號 | FR-05、BR-03 |
| 保留理由 | 對應分攤與金額顯示需求 |
| 待確認問題 | 是否保留多次重算版本 |

| 欄位 | 內容 |
| --- | --- |
| 類別名稱 | StatusLog |
| 類型 | Entity |
| 主要責任 1 | 記錄狀態變更歷程 |
| 主要責任 2 | 支援回溯與後續追蹤 |
| 重要屬性 | statusLogId、groupId、oldStatus、newStatus、changedAt |
| 主要操作 | appendLog() |
| 協作者 | Group |
| 來源編號 | FR-06、FR-07 |
| 保留理由 | 狀態轉移必須可追蹤 |
| 待確認問題 | 是否只記錄最終狀態 |

| 欄位 | 內容 |
| --- | --- |
| 類別名稱 | GroupJoinController |
| 類型 | Control |
| 主要責任 1 | 協調建立揪團與加入流程 |
| 主要責任 2 | 驗證流程條件與回傳結果 |
| 重要屬性 | currentAction、resultMessage |
| 主要操作 | createGroupFlow()、joinGroupFlow() |
| 協作者 | Group、JoinRecord、StatusLog |
| 來源編號 | UC-01、UC-03 |
| 保留理由 | 可避免畫面直接承擔業務規則 |
| 待確認問題 | 是否再拆成更細的服務類別 |

| 欄位 | 內容 |
| --- | --- |
| 類別名稱 | GroupJoinForm |
| 類型 | Boundary |
| 主要責任 1 | 接收發起或加入操作 |
| 主要責任 2 | 顯示結果與錯誤訊息 |
| 重要屬性 | formData、message |
| 主要操作 | submitCreateForm()、submitJoinForm() |
| 協作者 | GroupJoinController |
| 來源編號 | 原型畫面 |
| 保留理由 | 負責使用者互動與輸入輸出 |
| 待確認問題 | 是否與 UI 元件耦合過深 |

| 欄位 | 內容 |
| --- | --- |
| 類別名稱 | SplitCalculator |
| 類型 | Control |
| 主要責任 1 | 依據分攤規則計算每人金額 |
| 主要責任 2 | 防止分攤邏輯散落於畫面裡 |
| 重要屬性 | totalAmount、memberCount |
| 主要操作 | calculateShare() |
| 協作者 | SplitResult、JoinRecord |
| 來源編號 | FR-05、BR-03 |
| 保留理由 | 可把分攤計算與展示分開 |
| 待確認問題 | 是否需要保留多版本結果 |

### E. 類別明細

| 類別 | 類型 | 責任摘要 | 重要屬性 | 主要操作 | 來源 |
| --- | --- | --- | --- | --- | --- |
| Group | Entity | 保存揪團資料與狀態 | groupId、status、maxPeople | createGroup()、updateStatus() | FR-01、FR-03 |
| User | Entity | 代表參與者 | userId、name | joinGroup() | FR-04 |
| JoinRecord | Entity | 記錄加入歷史 | joinId、joinedAt、status | recordJoinEvent() | FR-04、BR-02 |
| SplitResult | Entity | 保存分攤結果 | splitId、shareAmount | calculateSplit() | FR-05 |
| GroupJoinController | Control | 協調流程與規則 | currentAction、resultMessage | joinGroupFlow() | UC-03 |
| GroupJoinForm | Boundary | 接收與顯示操作 | formData、message | submitCreateForm() | 原型 |

### F. 類別關係與多重性

| 類別 A | 關係 | 類別 B | A 端多重性 | B 端多重性 | 業務規則或模型依據 |
| --- | --- | --- | --- | --- | --- |
| Group | 擁有 | JoinRecord | 1 | 0..* | 一個揪團可有多筆加入紀錄 |
| User | 參與 | JoinRecord | 1 | 0..* | 一位使用者可有多筆加入紀錄 |
| Group | 產生 | SplitResult | 1 | 0..* | 一個揪團可產生多筆分攤結果 |
| JoinRecord | 對應 | SplitResult | 0..1 | 1 | 每次加入可對應一筆分攤結果 |
| Group | 記錄 | StatusLog | 1 | 0..* | 一個揪團可有多筆狀態歷程 |

關係判斷：

```text
這是一般關聯、聚合、組合或一般化：一般關聯
整體移除時，部分是否失去存在意義：是，JoinRecord 仍需保留歷史
部分是否可同時屬於多個整體：否，每筆 JoinRecord 只屬於一個 Group
子類別是否真的是父類別的一種：否
最後決定：採用一般關聯與關聯類別
理由：加入紀錄與狀態記錄與揪團之間是事件與歷史關係，不適合用一般化
```

### G. BCE 責任檢核

| 使用案例步驟 | 輸入或事件 | Boundary（邊界）責任 | Control（控制）責任 | Entity（實體）責任 | 問題與修正 |
| --- | --- | --- | --- | --- | --- |
| 建立揪團 | 填寫表單 | 接收輸入並顯示結果 | 協調建立流程與檢查規則 | Group、StatusLog | 無直接把規則塞入畫面 |
| 瀏覽看板 | 讀取活動列表 | 顯示卡片與剩餘名額 | 讀取 Group 資料並組裝畫面 | Group | 由控制類別整理資料 |
| 加入揪團 | 按下加入按鈕 | 接收加入請求與顯示成功或失敗 | 驗證狀態與建立 JoinRecord | Group、JoinRecord | 避免直接改變狀態 |
| 分攤計算 | 點擊分攤 | 顯示計算結果 | 呼叫 SplitCalculator | SplitResult | 將計算邏輯移出畫面 |
| 取消加入 | 點擊取消 | 顯示取消結果 | 檢查合法性與更新狀態 | JoinRecord、Group | 不允許直接把狀態改為任意值 |

### H. 類別圖完成檢查

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

### I. 循序圖範圍卡

#### 循序圖 1：主要流程

| 欄位 | 內容 |
| --- | --- |
| 循序圖編號 | SEQ-01 |
| 使用案例 | 建立揪團與顯示看板 |
| 流程類型 | 主要 |
| 前置條件 | 使用者已登入，尚未建立揪團 |
| 參與者 | 團主、GroupJoinForm、GroupJoinController、Group |
| 邊界類別 | GroupJoinForm |
| 控制類別 | GroupJoinController |
| 實體類別 | Group、StatusLog |
| 成功或失敗結果 | 建立成功並顯示可加入的卡片 |
| 對應後置條件 | Group 狀態為 open，卡片顯示正確 |

#### 循序圖 2：替代或例外流程

| 欄位 | 內容 |
| --- | --- |
| 循序圖編號 | SEQ-02 |
| 使用案例 | 加入揪團時額滿或重複加入 |
| 流程類型 | 替代／例外 |
| 前置條件 | 使用者已看到看板，Group 已存在 |
| 參與者 | 使用者、GroupJoinForm、GroupJoinController、Group、JoinRecord |
| 邊界類別 | GroupJoinForm |
| 控制類別 | GroupJoinController |
| 實體類別 | Group、JoinRecord |
| 成功或失敗結果 | 顯示額滿或重複加入警告 |
| 對應後置條件 | 不建立新的加入紀錄 |

### J. 循序圖訊息表

| 順序 | 發送者 | 接收者 | 訊息 | 輸入 | 回傳 | 對應類別操作 | 對應流程步驟 |
| ---: | --- | --- | --- | --- | --- | --- | --- |
| 1 | 團主 | GroupJoinForm | 提交建立揪團表單 | title、destination |  | submitCreateForm() | 1 |
| 2 | GroupJoinForm | GroupJoinController | 呼叫建立流程 | 表單資料 |  | createGroupFlow() | 2 |
| 3 | GroupJoinController | Group | 建立 Group 物件 | 基本資料 |  | createGroup() | 3 |
| 4 | Group | StatusLog | 記錄初始狀態 | open |  | appendLog() | 4 |
| 5 | GroupJoinController | GroupJoinForm | 回傳建立結果 | 成功 | 建立成功 |  | 5 |
| 6 | 使用者 | GroupJoinForm | 提交加入請求 | groupId |  | submitJoinForm() | 6 |
| 7 | GroupJoinController | Group | 檢查名額與狀態 | groupId | 是否可加入 | updateStatus() | 7 |
| 8 | GroupJoinController | JoinRecord | 建立加入紀錄 | userId、groupId | 建立成功 | recordJoinEvent() | 8 |

### K. alt 與 loop 片段檢查

| 片段 | 守衛或重複條件 | 來源規則 | 結果或終止條件 | 是否可測試 |
| --- | --- | --- | --- | --- |
| alt 分支 1 | 若人數未達上限 | FR-03 | 建立加入紀錄並顯示成功 | 是 |
| alt 分支 2 | 若已達上限 | FR-03 | 顯示額滿警告 | 是 |
| alt 其他分支 | 若重複加入 | BR-02 | 拒絕建立新紀錄 | 是 |
| loop | 重新計算剩餘名額直到流程結束 | FR-03 | 結束時顯示最新名額 | 是 |

### L. 狀態圖建模對象選擇

| 問題 | 回答 |
| --- | --- |
| 建模類別 | Group |
| 為什麼具有重要生命週期 | 其狀態會影響是否可加入、是否可關閉與是否可重新開放 |
| 不同狀態是否允許不同操作 | 是，open 可加入，full 需顯示額滿，closed 不可再加入 |
| 哪些規則限制狀態轉移 | 名額達到上限、取消加入、團主關閉活動 |
| 狀態錯誤會造成什麼風險 | 會讓畫面與資料狀態不一致 |
| 對應需求與使用案例 | FR-03、FR-06、UC-03 |

### M. 狀態轉移表

| 目前狀態 | 事件 | 守衛條件 | 動作 | 下一狀態 | 需求或規則 | 測試編號 |
| --- | --- | --- | --- | --- | --- | --- |
| open | 加入成功且人數未滿 | currentMemberCount < maxPeople | 更新 currentMemberCount | open | FR-03 | TC-S-01 |
| open | 加入成功且人數達到上限 | currentMemberCount = maxPeople | 更新 status | full | FR-03 | TC-S-02 |
| full | 取消加入 | 有人退出 | 釋出名額 | open | BR-02 | TC-S-03 |
| full | 關閉揪團 | 團主關閉 | 更新 status | closed | FR-06 | TC-S-04 |
| closed | 嘗試加入 | 無 | 拒絕操作 | closed | FR-06 | TC-S-05 |

### N. 非法狀態轉移

| 目前狀態 | 嘗試事件 | 為何不允許 | 預期系統反應 | 測試編號 |
| --- | --- | --- | --- | --- |
| closed | 再次加入 | 已關閉不可再加入 | 顯示不可加入 | TC-E-01 |
| full | 直接改為 closed 而不經過關閉流程 | 狀態需經由合法事件 | 拒絕非法狀態變更 | TC-E-02 |

### O. 跨模型一致性矩陣

| 情境或規則 | 需求 | 使用案例 | ERD（實體關係圖） | Class（類別圖） | Sequence（循序圖） | State（狀態圖） | 實作 | 測試 | 結論 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 建立揪團 | FR-01 | UC-01 | 有 Group 實體 | Group、GroupJoinController | SEQ-01 | open | 已有表單與建立流程 | TC-01 | 一致 |
| 加入揪團 | FR-04 | UC-03 | 有 JoinRecord 實體 | JoinRecord、Group | SEQ-02 | open/full | 已建立加入紀錄 | TC-02 | 一致 |
| 滿額防呆 | FR-03 | UC-03 | 有 maxPeople 與 status | Group | SEQ-02 | full | 需確保畫面與資料同步 | TC-03 | 部分一致 |
| 分攤計算 | FR-05 | UC-04 | 有 SplitResult 實體 | SplitResult、SplitCalculator | SEQ-03 | 無 | 已有計算邏輯但未完全整合 | TC-04 | 部分一致 |
| 取消加入 | FR-04、BR-02 | UC-05 | 有 JoinRecord 狀態 | JoinRecord | SEQ-04 | open/closed | 待補完整處理 | TC-05 | 待確認 |

### P. 一致性問題紀錄

| 問題編號 | 症狀 | 模型依據 | 最可能原因 | 修正位置 | 驗證方式 | 狀態 |
| --- | --- | --- | --- | --- | --- | --- |
| PI-01 | 目前畫面顯示名額與 Group.status 可能不同步 | FR-03、SEQ-02 | 狀態更新與資料顯示分離 | Group、GroupJoinController | 回歸測試與介面驗證 | 待修正 |
| PI-02 | 取消加入後仍無完整歷史紀錄 | BR-02、JoinRecord | 歷史保存與取消狀態尚未完整定義 | JoinRecord、StatusLog | 狀態轉移測試 | 待確認 |
| PI-03 | 分攤結果未與最新加入人數同步 | FR-05、SplitResult | 計算邏輯與資料更新時機未整合 | SplitCalculator、SplitResult | 測試與手動驗證 | 待修正 |

### Q. 更新後的模型驅動實作任務書

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
```

### R. 程式碼代理執行紀錄

| 次數 | 目的 | 提示詞摘要 | 產出摘要 | 採用 | 不採用與原因 | 人工修正 | 對應提交 |
| ---: | --- | --- | --- | --- | --- | --- | --- |
| 1 | 比對模型與原型 | 讀取 FR、ERD 與目前切片 | 建議把狀態由畫面改為 Group 控制 | 是 | 否 | 將規則移到 GroupJoinController | 未提交 |
| 2 | 檢查加入流程 | 比對 JoinRecord 與 Group 之間的責任 | 建議建立 JoinRecord 前先檢查狀態 | 是 | 否 | 保留原有介面流程 | 未提交 |
| 3 | 檢查分攤邏輯 | 比對分攤與加入數量 | 建議把計算移到 SplitCalculator | 是 | 否 | 先維持目前顯示結果 | 未提交 |

### S. 第四切片或重構紀錄

| 項目 | 內容 |
| --- | --- |
| 選擇 | 第四個可運作切片 |
| 對應需求 | FR-03、FR-04、FR-05 |
| 對應模型 | Group、JoinRecord、SplitResult、StatusLog |
| 原問題或新情境 | 加入後需顯示滿額與分攤結果 |
| 修改範圍 | 看板卡片、加入流程與分攤結果展示 |
| 可操作起點 | 建立一個揪團後加入 3 位使用者 |
| 可觀察結果 | 名額、狀態與分攤金額同步顯示 |
| 狀態變化 | open → full |
| 已知限制 | 候補與取消後歷史仍待確認 |
| 提交識別 | 尚未提交 |

### T. 主要、替代與狀態轉移測試

| 測試編號 | 類型 | 前置資料與狀態 | 操作 | 預期互動 | 預期結果狀態 | 實際結果 | 通過／失敗 | 證據 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| TC-01 | 主要 | Group open、名額未滿 | 建立揪團 | 顯示成功 | Group.status=open | 待驗證 | 待測 | 觀察畫面 |
| TC-02 | 主要 | Group open、有空位 | 加入揪團 | 建立 JoinRecord | open | 待驗證 | 待測 | 觀察畫面 |
| TC-03 | 替代／例外 | Group full | 再次加入 | 顯示額滿 | full | 待驗證 | 待測 | 觀察畫面 |
| TC-04 | 替代／例外 | 重複加入 | 重複提交 | 顯示已加入 | open/full | 待驗證 | 待測 | 觀察畫面 |
| TC-05 | 狀態轉移 | open | 加入達上限 | 狀態更新 | full | 待驗證 | 待測 | 觀察畫面 |
| TC-06 | 狀態轉移 | full | 取消加入 | 狀態重新開放 | open | 待驗證 | 待測 | 觀察畫面 |
| TC-07 | 狀態轉移 | full | 關閉揪團 | 狀態變 closed | closed | 待驗證 | 待測 | 觀察畫面 |
| TC-08 | 非法轉移 | closed | 再次加入 | 拒絕操作 | closed | 待驗證 | 待測 | 觀察畫面 |
| TC-09 | 非法轉移 | full | 直接修改狀態 | 拒絕非法轉移 | full | 待驗證 | 待測 | 觀察畫面 |

### U. 錯誤診斷與重測紀錄

```text
目前看到的症狀：加入後畫面顯示名額與 Group.status 可能不同步。
最可能的原因：狀態更新與畫面更新被放在不同層級，責任未完全分離。
涉及的需求、類別、訊息或狀態：FR-03、Group、JoinRecord、GroupJoinController、open/full。
下一個可驗證的修正步驟：將狀態更新與畫面同步邏輯移到 GroupJoinController 或 Group。
修正內容：以 Group 提供 updateStatus()，由控制類別統一呼叫並回傳結果。
重測方式：建立揪團後加入至額滿，觀察狀態與畫面是否同步。
重測結果：待執行。
保留的證據：目前畫面與狀態模型的對照記錄。
```
