# ERD 自我檢核與基數檢查

## I. ERD 自我檢核

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