# 設計基準與架構限制 (Design Baseline & System Constraints)

## A. 基準版本

| 項目 | 版本／日期／提交識別 | 連結 | 已確認限制 |
| --- | --- | --- | --- |
| SRS v1 | SRS v1 (115/07/27) | `docs/SRS_v1.md` | 以 FR-01~07、BR-01~03 為基準 |
| 使用案例 | UC-01 至 UC-06 (115/07/27) | `docs/UseCases.md` | 涵蓋發起、瀏覽、加入、計算、取消 |
| 類別圖 | Class Diagram v1 (115/07/27) | `docs/ClassDiagram.md` | 包含 Group, User, JoinRecord, SplitResult, StatusLog 等 |
| 循序圖 | SEQ-01 ~ SEQ-04 (115/07/27) | `docs/SequenceDiagrams.md` | 涵蓋建立、加入、滿額處理與分攤計算 |
| 狀態圖 | Group Life Cycle v1 (115/07/27) | `docs/StateDiagram.md` | Group 生命週期：open, full, closed |
| 目前實作 | 切片 4 (Commit: `a3f892c`) | GitHub `main` 分支 | 以假資料與前端互動為基礎，未連線真實 DB |

---

## U. 架構與部署的 8/3 補充範圍

### 本版已確認限制
| 編號 | 限制 | 來源 | 對本版影響 |
| --- | --- | --- | --- |
| C-01 | 目前全系統採用Mock/假資料與前端記憶體狀態運作，未連接真實資料庫。 | 7/27 基準 | 資料重整後會恢復預設值，無法跨 Session 持久化。 |
| C-02 | 認證機制採用模擬 Identity (User Role)，未整合校園單一簽入 (SSO)。 | SRS v1 待確認事項 | 使用者切換暫時由前端測試選單切換。 |

### 8/3 待確認問題
| 編號 | 問題 | 為何需要確認 | 需要的證據或決策者 | 影響章節 |
| --- | --- | --- | --- | --- |
| OQ-01 | 後端框架與資料庫選型 (例如：Node.js vs Spring Boot, PostgreSQL vs MongoDB) | 影響正式部署架構與實體 ERD 轉移 | 指導老師與技術組長決策 | SDD v1 部署與系統脈絡 |
| OQ-02 | 校園 SSO 介接方式與個資授權範圍 | 需確定是否能取得學生學號與真實姓名 | 校方資訊處 API 規範 | SDD v1 安全與權限設計 |
