# [專案名稱] 專案規劃規格書 (PRD)

## 1. 專案說明與系統目的 (System Purpose)
* **核心痛點**：[描述要解決什麼問題]
* **目標受眾**：[系統主要給誰使用]

## 2. 商業驗證與假設 (Problem-Solution Fit)
* **核心假設**：[例如：如果我們提供 X 功能，目標受眾願意留信箱/付費]
* **驗證指標 (KPI)**：[例如：Landing page 轉換率達 5%，或取得 100 個 Waitlist]

## 3. 系統範圍 (System Scope)
* [Feature 1]
* [Feature 2]

## 4. 角色權限與操作邊界 (Authorization Model)
| 角色 | 權限內容 | 邊界限制 |
| :--- | :--- | :--- |
| Admin | 全系統設定、使用者管理 | 不可竄改 Audit Log |
| User | 一般功能操作、個人資料維護 | 僅能存取所屬單位的資料 |

## 5. 作業流程與非同步設計 (Workflow & Scalability)
### 5.1 [Workflow Name]
1. [Step 1]
2. [Step 2 - 耗時的 AI 任務應拋入 Message Queue 進行非同步處理]

## 6. 技術架構與限制 (Architecture Constraints)
* **前端開發**：React / Next.js 等...
* **後端與資料庫**：Node.js / PostgreSQL / Supabase 等...
* **API 契約鎖定 (Contract Lock)**：使用 OpenAPI / JSON Schema 嚴格規範，未經核准前，嚴禁 AI 擅自修改 API Schema 或回傳格式。

## 7. 資安規範與 AI 安全層 (Security & AI Safety)
* **資料治理 (Data Governance)**：敏感 PII 資料存儲必須加密或遮罩。
* **AI 安全層 (Safety Layer)**：輸入端實施防 Prompt Injection 檢查，輸出端檢查 Hallucination。
* **操作稽核 (Observability)**：保留所有關鍵操作與 API 調用的結構化 Audit Log。

## 8. 資料庫 Schema (Database Design)
### 8.1 核心資料表
* **`users`**：使用者帳號與權限層級。
* **`audit_logs`**：系統與 AI 操作日誌。