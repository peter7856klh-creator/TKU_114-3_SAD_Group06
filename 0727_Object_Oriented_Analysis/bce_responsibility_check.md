# BCE 責任檢核表 (BCE Responsibility Check)

## G. BCE 責任檢核

| 使用案例步驟 | 輸入或事件 | Boundary（邊界）責任 | Control（控制）責任 | Entity（實體）責任 | 問題與修正 |
| --- | --- | --- | --- | --- | --- |
| 建立揪團 | 填寫表單 | 接收輸入並顯示結果 | 協調建立流程與檢查規則 | Group、StatusLog | 無直接把規則塞入畫面 |
| 瀏覽看板 | 讀取活動列表 | 顯示卡片與剩餘名額 | 讀取 Group 資料並組裝畫面 | Group | 由控制類別整理資料 |
| 加入揪團 | 按下加入按鈕 | 接收加入請求與顯示成功或失敗 | 驗證狀態與建立 JoinRecord | Group、JoinRecord | 避免直接改變狀態 |
| 分攤計算 | 點擊分攤 | 顯示計算結果 | 呼叫 SplitCalculator | SplitResult | 將計算邏輯移出畫面 |
| 取消加入 | 點擊取消 | 顯示取消結果 | 檢查合法性與更新狀態 | JoinRecord、Group | 不允許直接把狀態改為任意值 |