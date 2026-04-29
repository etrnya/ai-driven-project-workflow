[English](README.md) | [繁體中文](README_zh-TW.md)

# 🚀 全端 + AI 專案自動化調用流程 (AI-Driven Fullstack Project Workflow)

這是一套為 AI 代理 (AI Agent) 量身打造的工業級軟體開發生命週期 (SDLC) 框架。透過這套流程，使用者無需手動逐一調用技能 (Skills)，AI 將扮演技術架構師與開發團隊，引導使用者完成從「市場驗證」到「CI/CD 部署」的完整閉環。

---

## 💡 誕生緣由 (Why This Exists)

這個框架的誕生，來自兩個真實的切身痛苦：

**1. 好輪子沒辦法在對的時機用上**
在學會使用 AI Skills（技能模組）之後，我發現網路上已經有非常多品質優秀、由前人打造的 Skills——涵蓋架構設計、資安審查、前端開發、RAG 等各個領域。但問題是：**我記不住什麼時候該用哪個、也想不起調用的關鍵詞。** 這些好輪子就這樣靜靜躺在資料夾裡，沒有被好好利用。

因此，我的想法是：**在這些好輪子之上，再造一個更大的調度層。** 讓 AI 能夠理解我目前在哪個開發階段，並在正確的時機，自動調用應該使用的技能——而不是讓我靠記憶力手動觸發。

**2. C 槽大屠殺事件 (CASCADE-DELETE-RECURSIVE-0426)**
在使用 Antigravity 開發的過程中，曾發生過 AI 執行 PowerShell 指令時因萬用字元誤判，導致大規模刪除本機檔案的慘痛事故。那次事件讓我深刻意識到：**如果沒有強制的隔離防護與頻繁備份機制，AI 輔助開發本身就是一顆不定時炸彈。**

這個框架將這兩個動機化為具體規則，讓每一個非技術背景的人，都能在安全的環境下，有條不紊地用 AI 打造出具備工業級品質的軟體產品。

---

## 🎯 核心設計理念
1. **Agent 主導與 Human-in-the-loop**：AI 掌握推進節奏，但 **AI 只是工程師，不是 CTO**。架構設計、DB 變更、刪除操作等重大決策，必須設置「人工審批閘門 (Gate)」確認後才可放行。
2. **謀定而後動 (驗證先行)**：強制在寫程式前進行 Problem-Solution Fit 驗證與產出 PRD，避免做出「架構完美但沒人要用」的工業垃圾。
3. **極致防禦與可觀測性**：內建 Docker 隔離防護、資安審查，並強調系統與 AI 的可觀測性 (Observability) 與成本控制，確保系統上線後「不爆預算、抓得到蟲」。

---

## 🗺️ 核心八大階段 SOP

### Phase 0: 問題定義 (Problem Framing) 🔍 *[基石]*
* **觸發條件**：使用者提出初步想法。
* **AI 任務**：不急著想功能。先釐清「使用者是誰」、「痛點是什麼」、「現在怎麼解決」以及「為什麼現有方案不夠好」，大幅提升後續商業驗證的成功率。

### Phase 1: 概念收斂 (Concept Clarification)
* **觸發條件**：問題定義完成。
* **AI 任務**：詢問 1~2 個關鍵問題，釐清系統核心目標與主打功能。

### Phase 2: 市場探索與功能清單 (Market Research & Feature Discovery)
* **觸發條件**：確認系統核心目標後。
* **AI 任務**：主動使用**網路搜尋 (Web Search)** 進行競品分析，列出「核心用途」與「標準功能清單 (Feature List)」。
* **💡 彈性跳過機制**：如果系統僅為解決自身內部需求，使用者可宣告進入 **風險自負模式 (Risk Acceptance Mode)**，跳過市調與商業驗證直接進入 Phase 3。

### Phase 2.5: 商業驗證閉環 (Problem-Solution Fit) 🌟 *[關鍵新增]*
* **觸發條件**：Feature List 確認。
* **AI 任務**：不急著寫 Code。先產出「假設清單 (Hypothesis)」，並設計 MVP 驗證方案（例如：Landing Page 企劃、假 CTA 收信等待名單、Reddit/社群測試貼文），在投入開發成本前，證明這東西值得寫。

### Phase 3: 產品路線圖規劃 (Roadmap Planning)
* **觸發條件**：驗證方案與核心功能確認。
* **AI 任務**：將功能劃分優先級，制定 Roadmap，明確定義 MVP 範圍。並依照以下**砍功能標準 (Feature Triage Criteria)** 強制過濾，避免 MVP 無限膨脹：
  * ❌ **砍掉進 Backlog**：預估實作超過 3 天、不影響核心使用流程、或只有少數極端情境才會用到的功能。
  * ✅ **留在 MVP**：使用者無法沒有它、失去它會讓產品根本無法使用的功能。
  * 💡 **原則**：寧可做少做精，也不要做多做爛。MVP 的目標是「能用」，不是「完整」。

### Phase 4: 系統架構、資安與維運設計 (Architecture & Ops) 🛡️ *[深度強化]*
* **觸發條件**：MVP Roadmap 確認。
* **AI 任務**：
  1. **核心架構與 DB Schema**：推薦框架並設計資料庫，加入**資料治理 (Data Governance)**（如：PII 個資處理、資料保留期限、Audit Log 誰看過什麼）。
  2. **權限與角色模型 (Authorization Model)**：設計嚴謹的 RBAC/ABAC 機制，明確定義操作邊界。
  3. **非同步與擴展性 (Scalability & Backpressure)**：配置 Docker 專屬沙盒。導入 Message Queue 處理非同步 AI 任務，並加入 **任務隔離 (Job Isolation)** (設定 timeout/retry 策略，確保單一任務掛掉不拖垮 Worker) 與 **降壓機制 (Backpressure)** (當 Queue 過長時拒絕請求或降級服務，防止系統過載崩潰)。
  4. **可觀測性與目標 (Observability & SLO/SLA)**：除了 Logging, Tracing, Metrics 外，制定明確的 **服務等級目標 (SLO)** (如 99.9% uptime, P95 延遲 < 2s)，並綁定 **告警機制 (Alerting)**，當指標異常時主動通知。
  5. **AI 行為審計 (AI Audit Trail)**：除了資料層級的 Log，必須專門記錄 AI 的關鍵決策軌跡（如：生成內容依據、工具調用紀錄），確保 AI 行為可追溯與符合法規。
  6. **AI 成本控制與容錯 (Cost Governance)**：設立 Cache，並強制加上 **預算上限防護 (Budget Guardrail)**，達標即自動降級模型或暫停服務。
  7. **濫用防護 (Rate Limiting)**：在 API 層級強制加入速率限制與配額 (Quota)，防刷爆與被當免費算力。
  8. **AI 安全防護層 (AI Safety Layer)**：加入防 Prompt Injection 過濾、Tool 使用白名單機制，以及敏感資料遮罩 (PII masking)。
  9. **外部依賴風險管理 (Dependency Risk)**：針對 LLM API 或第三方套件，制定 Fallback 策略，落實套件版本鎖定 (lockfile) 與定期安全更新 (dependency patching)。
  10. **API 契約鎖定與版本化 (Contract Locking & Versioning)**：使用 OpenAPI 鎖定規格。落實 **API Versioning** 與 **Prompt Versioning**。**嚴禁 AI 擅自修改欄位名稱或格式**。
  11. **設定管理 (Configuration Management)**：區隔 Dev/Staging/Prod 設定，提供 Default Fallback，**嚴禁將環境依賴硬編碼 (Hardcode)**。
  12. **架構建議人工核查 (Architecture Sanity Check)** 🔍：AI 提出的架構方案在人工 Approve 前，**必須逐一核查以下項目**，防止 AI 幻覺誤導整個專案方向：
      * 推薦的套件/框架是否真實存在？版本號是否為最新穩定版？
      * 引用的 API 或服務是否仍在維護，而非已棄用 (Deprecated)？
      * DB Schema 的欄位設計是否與 Glossary 定義一致，沒有自創新詞？
      * **驗證方式**：對 AI 說「請提供上述所有套件的官方文件連結」，若 AI 無法提供或連結失效，即為幻覺警告，需要重新確認。
  13. **本地化與多語言 (i18n / Internationalization)**：**此決策必須在架構階段就定案，後期再補代價極高。** 請在此階段明確回答以下問題：
      * 這個產品是否需要支援多語言或多地區？（即使現在只做一種語言，也要預先設計好擴充路徑）
      * 若需要：選定 i18n 框架（如 `next-intl`、`i18next`）、定義語言資源的存放結構，並確保 **所有 UI 文字、錯誤訊息、Email 範本均不能硬編碼在程式碼中**。
      * 若不需要：在 PRD 中明確記錄「目前為單語言，未來若需擴充需重構 UI 字串層」。

### Phase 5: 產出 PRD 規格書 (PRD Generation)
* **觸發條件**：架構設計經人工 Approve。
* **AI 任務**：整合 Phase 1 ~ 4 討論，輸出一份極具水準的 Markdown 專案規劃規格書，**必須包含以下兩個固定章節**：
  * **專案動機 (Why This Exists)**：說明這個軟體為什麼存在、解決了誰的什麼問題、現有方案哪裡不夠好。這是給未來的使用者、貢獻者與自己看的「初心錨點」。
  * **授權聲明預留 (Licensing Placeholder)**：預先寫入本專案打算採用的授權條款（如 MIT），並提示 Phase 7 時需補齊完整的 `ATTRIBUTION.md`。

### Phase 5.5: 術語字典 (Glossary) 📖 *[對齊基石]*
* **觸發條件**：PRD 確認後、開始寫程式前。
* **AI 任務**：產出一份獨立的 `GLOSSARY_V1.md`，定義本專案所有專有名詞的**唯一、標準化**解釋。例如：
  * **業務概念**（如：「訂單 (Order)」vs「請求 (Request)」的邊界）
  * **資料實體**（如：`User` / `Account` / `Member` 各自指什麼）
  * **系統術語**（如：「任務 (Job)」、「事件 (Event)」、「工作流程 (Workflow)」的差異）
* **強制執行**：後續所有階段（架構設計、API 命名、資料庫 Schema、前端 UI 文字）**必須嚴格遵守此字典**，禁止 AI 在不同地方使用同義但不同名的詞彙。
* **動態更新**：每當引入新概念，必須先更新 Glossary 並遞增版本號（如 `GLOSSARY_V2.md`），再繼續開發。

### Phase 6: 敏捷開發與多層級測試 (Agile Implementation & Testing) 🧪 *[深度強化]*
* **觸發條件**：PRD 與 Glossary 確認後，**另開全新 AI 對話視窗**，依照以下 **Context 交接流程 (Context Switch Protocol)** 操作，確保 AI 帶著正確規則進入開發，而非從空白記憶重新猜測：
  1. 將 `PRD_Vx.md` **完整內容**貼入新對話的第一則訊息作為 System Prompt。
  2. 緊接著貼上 `GLOSSARY_Vx.md` 全文，告知 AI「以下是本專案所有術語的唯一定義，開發過程中必須嚴格遵守」。
  3. 若 PRD + Glossary 合計超過 8,000 字，則改貼**精簡摘要版**（由 AI 在 Phase 5 結束時一併產出 `PRD_SUMMARY_Vx.md`），避免爆掉 Context Window。
  4. 最後說：「請依照上述規格開始 Phase 6 開發，每次修改前先說明你的計畫，等我確認後再動手。」
* **AI 任務**：
  1. 在 Docker 隔離區內開發。
  2. 實踐**測試金字塔**：包含 Unit Test, Integration Test, E2E Test (Playwright/Cypress)。
  3. **AI 專屬評估 (AI Evaluation)**：加入 Prompt Regression Test、輸出品質檢查與幻覺基準測試 (Hallucination Baseline)。
  4. **測試資料策略 (Test Data)**：嚴格建立 Mock Data 與 Seed Data，絕對禁止在開發與測試環境使用真實的敏感 Production Data。

### Phase 7: 部署策略與知識沉澱 (Deployment & Documentation) 🚀 *[關鍵新增]*
* **觸發條件**：MVP 開發與測試通過。
* **AI 任務**：
  1. **CI/CD 建立**：規劃 GitHub Actions 等 pipeline，設置 Preview Deploy (PR 預覽)。
  2. **環境分離與進階部署 (Deployment Strategy)**：明確定義從 Staging 到 Production 的推進策略，並導入 **Blue-Green Deployment** 或 **Canary Release** 來降低上線風險。
  3. **金鑰管理 (Secrets Management)**：獨立規範，強制使用 Vault 或 GitHub Secrets，嚴禁在原始碼或 CI/CD 日誌中暴露敏感設定。
  4. **災難復原計畫 (Disaster Recovery Plan)**：設計 DB Migration Rollback、Feature Flag (快速開關功能) 以及定期備份。**強制要求定期執行備份還原演練 (DR Drill)**，確保備份檔案不是假安全感。
  5. **文件與知識沉澱 (Documentation Loop)**：強制 AI 產出可讀文件：自動生成 **API 文件 (OpenAPI)**、**系統架構圖 (System Diagram)**，並詳細記錄每一次的 **架構決策 (ADR, Architecture Decision Record)**，避免專案核心邏輯只存在 AI 的腦中。
  6. **ATTRIBUTION.md 產出**：掃描專案所有直接依賴（npm / pip / Cargo 等），產出 `ATTRIBUTION.md` 列出每個套件的名稱、版本、授權。確保本專案對外發布時符合所有第三方授權的標示義務。

### Phase 8: 上線後迭代閉環 (Post-Launch Feedback Loop) 🔄 *[持續演進]*
* **觸發條件**：產品正式上線後。
* **AI 任務**：產品上線不是終點，而是真正考驗的開始。建立以下持續迭代機制：
  1. **使用者回饋收集**：建立結構化的回饋管道（如 GitHub Issues 模板、站內回饋表單、或定期用戶訪談），確保真實聲音能被系統性地蒐集，而非消失在聊天記錄裡。
  2. **Bug 分類與優先級**：每週 AI 協助整理回報清單，依照「影響用戶數 × 嚴重程度」排序，確保有限的開發時間花在刀口上。
  3. **迭代回注流程 (Loop Back)**：新需求或重大 Bug 修復，必須從 **Phase 3 Roadmap** 重新評估優先級，而非直接跳進 Phase 6 寫 code，避免功能混亂累積技術債。
  4. **SLO 監控回顧**：每月對照 Phase 4 制定的 SLO 指標（uptime、P95 延遲等），若持續未達標，必須觸發架構檢討（回到 Phase 4）。
  5. **Skill 更新同步**：當 AI 工具或 Skills 有重大更新時，評估是否需要更新本框架的調用策略與 PRD 範本。

---

## 🛠️ 必備技能清單 (Prerequisites)
為確保此流程順利運行，必須在 AI 環境中安裝以下重點 Skills（依照用途分類）。
> 📦 **開箱即用**：本 GitHub 專案內已附帶 `skills/` 資料夾，包含了下方列出的所有必備技能。請將該資料夾內的所有內容複製到您 AI Agent 的專屬技能庫目錄中即可。

* **總控與規劃**：`project-spec-generator` (本流程核心控制器), `context-window-management`
* **體驗與設計**：`ui-ux-pro-max`, `frontend-design`, `form-cro`
* **架構與後端**：`senior-fullstack`, `database-design`, `backend-dev-guidelines`, `api-patterns`
* **資安與法規**：`cc-skill-security-review`, `backend-security-coder`, `frontend-security-coder`
* **前端與實作**：`frontend-developer`, `react-best-practices`, `react-patterns`
* **AI 與進階整合**：`rag-implementation`, `rag-engineer`, `langgraph`, `agent-evaluation`

---

## 🚀 給其他開發者的使用指南 (How to Use)

這套框架不僅僅是文件，更是一套 **「可執行的 AI Prompt 系統」**。

1. **匯入技能 (Import Skills)**：
   * 本專案的 `skills/` 目錄下包含了所有階段需要的核心知識與 Prompt。
   * 如果你使用支援自訂技能的 Agent (如 Antigravity)，請直接將資料夾複製進技能庫。
   * 如果你使用 **Cursor / Windsurf** 等 AI IDE，可以將這些 Markdown 內容納入你的 `.cursorrules` 或 System Prompt。
   * 如果你使用 **ChatGPT / Claude**，可在對話初始時，先上傳此 `README.md` 與對應階段的 skill 檔案作為前置 Context。

2. **啟動咒語 (The Trigger Prompt)**：
   在準備好環境後，只需對你的 AI 傳送以下起手式：
   > **「請閱讀 README.md，並啟動【全端+AI 專案自動化調用流程】，我想要做一個 [填寫你想做的系統或工具]。」**

3. **順流而下**：接下來，請放手讓 AI 主導，跟隨引導回答問題，享受架構師等級的開發體驗。

---

## 🆘 卡關處理指引 (Stuck? Here's What to Do)

就算不懂程式，在某個 Phase 被 AI 的問題卡住也很正常。這裡有兩種處理方式：

### 🆙 準則 A：求救指令（不清楚怎麼回答時）
直接對 AI 說以下任一句話：

> **「我不太懂這個問題，你能幫我解釋一下為什麼這個問題很重要嗎？然後給我一個不懂技術的人也能理解的範例。」**

> **「如果我沒有背景知識回答這個問題，你可以提供幾個選項讓我從中選擇嗎？」**

### ⏩ 準則 B：跳過機制（確實不知道、且目前不影響前進時）

> **「我目前對這個問題沒有答案，我想先用預設答案繼續進行，後面如果發現不對再調整。請幫我給一個最合理的預設假設並說明理由。」**

> ⚠️ **跳過原則**：每個 Phase 最多只跳過一次，且跳過的決定必須在 PRD 裡標註。如果每個 Phase 都在跳過，請回到 Phase 0 重新確認專案目標。

---

## ⚠️ 注意事項與極致安全守則 (Extreme Safety Rules)

> 💡 **鑑識教訓 (CASCADE-DELETE-RECURSIVE-0426)**：本流程導入最高防呆機制。

1. **🐳 強制 Docker 沙盒隔離 (FS Sandboxing)**：開發階段絕對禁止於本機全域執行，必須在容器內操作。
2. **🚫 PowerShell 萬用字元與指令防護**：
   * 永遠只能使用 `-LiteralPath` 進行檔案操作，避開 `[id]` 等路由命名導致的正則誤判。
   * **禁止指令拼接**：明文禁止 AI 使用 `\n` 拼接多個危險指令盲目重試。
3. **⛔ 人機分工邊界 (Human-in-the-loop Gate)**：架構變更、DB Schema 調整、高風險刪除與 Migration，必須暫停並由人工給予 `Approve` 才可放行。
4. **💾 頻繁的 Git 節點備份**：每完成小功能或重大重構前，強制執行 `git commit`。
5. **🛡️ 資安與敏感資訊**：嚴禁將 API Keys 寫入程式碼，落實 `.env` 管理。
6. **📄 檔案命名流水號規範**：所有 AI 產出的重要文件（PRD、Glossary、架構圖等）一律使用以下命名模板，每次修訂遞增版本號，禁止覆蓋舊版本：
   ```
   {filename}_V{versionSeq}.md
   ```
   範例：`PRD_V1.md` → `PRD_V2.md`、`GLOSSARY_V1.md` → `GLOSSARY_V2.md`

---

## 🤝 關於作者與協作聲明 (Credits)
本開源框架是由 **一個對程式一竅不通的普通人 (etrnya)** 與 **AI 代理 (Antigravity)** 共同經歷無數次除錯、真實事故鑑識 (Forensics) 與高階架構推演後，深度協作誕生之結晶。

etrnya 沒有寫過任何正式的軟體專案，也不懂程式語言——但透過與 AI 的高強度協作，依然能夠驅動出具備工業級品質的架構設計與開發流程。這正是這套框架最有力的存在意義：**你不需要是工程師，也能像工程師一樣思考、決策，並打造出真正可用的產品。**

我們將 SRE、資安、AI 防護與商業驗證的精華濃縮於此，希望能幫助所有對技術感到陌生的人，避開 AI 輔助開發的致命盲區，優雅且安全地打造出具備百萬級流量潛力的軟體產品。

---

## 📜 授權聲明 (Licensing & Attribution)

本專案的框架文件（`README`、Skill 文件等）採用 **MIT License** 授權開放使用。

本專案 `skills/` 目錄下打包的所有技能模組，均來自 **[antigravity-awesome-skills](https://github.com/antigravity-awesome-skills)** 策劃集合，其內容依原始來源分別受 **CC BY 4.0** 與 **MIT** 授權保護。

> Skills used in this project are sourced from [antigravity-awesome-skills](https://github.com/antigravity-awesome-skills), licensed under CC BY 4.0 and MIT.

完整的第三方來源與授權清單詳見 **[ATTRIBUTION.md](./ATTRIBUTION.md)**。