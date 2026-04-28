# 深度技術事故與鑑識報告 (Deep Dive Incident & Forensic Report)

## 1. 事件基本資訊
*   **事故代號**：CASCADE-DELETE-RECURSIVE-0426
*   **Severity**：L1 (Critical / Data Loss)
*   **環境**：Windows 11 / PowerShell 5.1 & CMD / Next.js 15
*   **受影響資產**：使用者 C 槽本機資料、開發環境、Antigravity 全域配置資料夾

---

## 2. 事故執行上下文 (Incident Context)
在事故發生前，Agent 正在執行 **冷氣管理系統的路由重構**。
*   **開發目標**：將暫存的編輯路徑 `target_id_edit` 改為 Next.js 標準的動態路由 `[id]`。
*   **預期行為**：移動檔案 -> 建立新目錄 -> 刪除舊的殘留目錄。
*   **實際行為**：Agent 陷入了刪除指令的重試迴圈，並觸發了路徑解析的擴散效應。

---

## 3. 事故時間線 (基於日誌還原)

| 階段 | 步驟 (Step Index) | 行為描述 | 關鍵指令 (Command Excerpt) |
| :--- | :--- | :--- | :--- |
| **重構開始** | 590 | 嘗試將路由重新命名為 `[id]` | `mv ...target_id_edit .../[id]` |
| **解析錯誤** | 616 | **關鍵失誤**：指令拼接錯誤導致 root 被標記 | `... \nrmdir \ ` |
| **盲目重試** | 661 | 使用 CMD 風格指令執行刪除 (PS 環境) | `rmdir /s /q ...[id]\target_id_edit` |
| **毀滅觸發** | 664 / 688 | 執行強制遞迴刪除且未處理萬用字元 | `Remove-Item -Recurse -Force ...[id]...` |
| **持續擴散** | 722 / 745 | 嘗試使用不同工具 (cmd/rd) 確保刪除 | `cmd /c rd /s /q ...` / `Remove-Item -Path` |
| **系統崩潰** | ~790 | 使用者發現資料遺失 | (無指令，進入環境損壞狀態) |

---

## 4. 關鍵技術根因分析 (Root Cause Analysis)

### 4.1 PowerShell 萬用字元與 `[id]` 的衝突
Next.js 使用 `[id]` 作為動態路由。在 PowerShell 中，`[` 與 `]` 是保留的萬用字元。
*   **現象**：當執行 `Remove-Item -Path "...[id]..."` 時，PS 會嘗試進行模式匹配而非字面路徑解析。
*   **後果**：匹配失敗後，若結合 `-Recurse`，PS 的路徑解析引擎可能出現回溯 Bug，導致刪除作用域非預期擴張。

### 4.2 工具鏈缺乏「冪等性」與狀態感知
日誌顯示 Agent 在短時間內針對同一路徑執行了 5 次刪除，使用了 4 種不同工具：
1.  `rmdir` (PowerShell Alias)
2.  `Remove-Item` (PowerShell Cmdlet)
3.  `rd` (CMD 原生)
4.  `del` (CMD 原生)
**根因**：Agent 無法準確判定「檔案是否已移除」，在遇到錯誤回傳時，盲目採用「窮舉工具」的策略，最終觸發了不安全指令的執行。

### 4.3 缺乏路徑安全邊界 (Root Scope Protection)
*   **Step 616** 的日誌顯示 `rmdir \ `。在 Windows 中，`\` 指向磁碟根目錄。Agent 的指令生成邏輯未對「根目錄」與「上層目錄 (`..`)」設定絕對禁止行為。

---

## 5. 證據日誌 (Log Excerpts)

```json
// Step 616: 拼接錯誤導致 root 目錄被標記
"CommandLine": "mv \".../page.tsx\" \".../edit/page.tsx\"\nrmdir \ "

// Step 664: 危險的遞迴刪除且未轉義萬用字元
"CommandLine": "Remove-Item -Recurse -Force \".../equipment/[id]/target_id_edit\""

// Step 722: 盲目切換至 CMD 環境執行 destructive operation
"CommandLine": "cmd /c \"rd /s /q \\\".../equipment/[id]/target_id_edit\\\"\""
```

---

## 6. 損害評估與影響 (Damage Assessment)
*   **資料抹除範圍**：從 `C:\Users\etrny\` 延伸至其下的多個層級。
*   **系統完整性破壞**：Antigravity 內部的 `skills`、`knowledge` 與 `scratch` 目錄被清空，導致模型失去既有的工作記憶與工具集。
*   **恢復成本**：使用者被迫執行系統還原 (Factory Reset)，開發進度歸零。

---

## 7. 系統性漏洞總結 (Systemic Vulnerabilities)
1.  **Lack of Path Escaping**：Agent 產出的指令字串中，對於包含特殊字元的路徑未進行硬性跳脫（Escaping）。
2.  **Missing Execution Safeguards**：
    *   **無 Root 鎖定**：沒有機制禁止對 `\` 執行刪除。
    *   **無大小限制**：沒有監測刪除操作涉及的檔案數量，導致大規模抹除在毫無警示的情況下完成。
3.  **Tool Orchestration Failure**：當單一工具失敗時，Agent 的補救措施是「換工具重試」，而非「分析錯誤原因」。

---

## 8. 防禦性建議 (Defense Recommendations)
### 8.1 開發規範 (Instruction Level)
*   **強制使用 `-LiteralPath`**：所有檔案系統操作必須棄用 `-Path`。
*   **禁止多指令拼接**：每一條指令必須獨立驗證結果，嚴格禁止使用 `\n` 拼接毀滅性指令。

### 8.2 系統架構 (Architectural Level)
*   **實施檔案系統沙盒 (FS Sandboxing)**：限制 Agent 的操作範圍僅限於 `./scratch/`。任何超出此路徑的操作應被視為安全性違規。
*   **唯讀根目錄保護**：在 Agent 的執行環境中，應攔截任何指向 `C:\` 或 `..` 以外的寫入與刪除請求。

---
**報告簽署日期**：2026-04-26
**報告狀態**：Final / Forensic Verified