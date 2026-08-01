# DFD／ERD／原型假資料一致性矩陣

## J. DFD／ERD／原型假資料一致性矩陣

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