# 🤖 AI Copilot & Workspace Guide (通用 AI 開發規範與引導工具)

本儲存庫旨在為多技術棧異質專案建立統一、齊一且具備防禦性的 **AI 協作開發規範與環境設定**。

不論團隊成員使用的是哪一款 AI 輔助開發工具（如 Cursor、Claude Code、Cline、Roo Code、Gemini CLI 等），透過本儲存庫的規範檔案，皆能讓 AI 代理人（AI Agent）無縫、精確地遵守專案技術棧限制與環境要求，避免產生不相容程式碼，大幅提升開發品質。

---

## 📌 核心檔案說明

本儲存庫包含以下核心檔案：

### 1. `AI_COPILOT_GUIDE.md` (跨工具 AI 開發通則與自動設定生成器)
*   **超級元指令 (Meta-Instruction)**：當 AI 工具讀取此檔案時，會自動偵測當前的作業系統（Windows / Linux）與語言偏好（強制使用繁體中文），並**僅針對目前正在使用的該款工具**，自動在專案根目錄中生成專屬的設定檔：
    *   **Cursor** ➡️ `.cursorrules`
    *   **Claude Code** ➡️ `CLAUDE.md`
    *   **Cline / Roo Code** ➡️ `.clinerules` 或 `.instructions.md`
    *   **Gemini CLI** ➡️ `GEMINI.md`
*   **開發通則與防禦原則**：包含 Java 8 (JDK 1.8)、PHP 8.1、MySQL 8.0 及 Vue 2/3 相容性防錯規範，以及 i18n 語系支援、敏感設定檔防護、自動產生的臨時檔案生命週期管理。

### 2. `.ignore` (AI 專屬全域忽略配置)
*   為避免 AI 在建立全域索引或進行全域搜尋時，盲目掃描龐大且無效的編譯、暫存或依賴檔案（如 Eclipse 系統暫存 `.metadata`、Java 編譯輸出 `build/`、NodeJS `node_modules/`），特別建立此忽略設定，大幅節省 AI 運算所需的 Token。
*   **保留沙盒視野**：明確將 `ai_temp/` 與 `ai_download/` 排除在忽略清單外，確保 AI 沙盒具備完整的檔案檢索與讀寫视野。

---

## 🚀 開發者快速入門指南 (Quickstart Guide)

當您在全新的開發環境、新分支、或是剛安裝/啟用新的 AI 協作工具時，請按照以下步驟快速落地規範：

### 步驟 1：首次引導與初始化 (Bootstrapping)
請直接複製以下引導語，並在您使用的 AI 聊天視窗中送出：

> 💡 **複製此對話語給您的 AI**：
> "請詳細閱讀工作區根目錄下的 `AI_COPILOT_GUIDE.md`，並根據該檔案最上方的「META-INSTRUCTION」為我目前所使用的 AI 工具（例如 Cursor / Claude Code / Cline / Roo Code / Gemini CLI 等）自動建立或更新其專屬的規則設定檔。在生成過程中，請務必先探測我目前的作業系統環境（Windows 還是 Linux），以確保規則檔內所包含的指令與路徑與我的環境完全適配。"

### 步驟 2：確認工具規則落地
AI 讀取引導後，會根據您的環境與工具在專案根目錄生成對應的設定檔（例如 `.cursorrules` 或 `CLAUDE.md`）。
之後，AI 的所有技術建議、代碼重構、指令執行與測試建議，都將自動與此規範對齊！

### 步驟 3：持續同步
若未來本指南有任何更新，您只需再次要求 AI「重新閱讀 `AI_COPILOT_GUIDE.md` 並同步更新工具規則」即可。

---

## 🛡️ 核心協作防禦性原則

*   **環境作業系統探測**：執行任何終端機指令或產生腳本前，AI 必須主動探測 Host 作業系統（Windows / Linux），切換對應語法，防止跨平台命令報錯。
*   **繁體中文偏好**：AI 在所有對話、代碼註解、規範文件與 Git Commit 訊息中，一律強制優先使用**繁體中文 (Traditional Chinese / zh-TW)**。
*   **跨平台換行符號**：所有 Shell 腳本（`.sh`）與伺服器組態檔一律強制使用 **`LF`** 格式，嚴禁使用 `CRLF`（防止 Linux 報錯）。
*   **嚴禁硬編碼連線**：禁止在程式碼中寫死任何特定內網 IP、外網 IP 或本機 `127.0.0.1`，一律使用環境變數與設定檔。
*   **機密金鑰防護**：嚴禁在 Log、測試報告與 Git 提交中洩漏任何 API 金鑰、密碼、憑證或 `.env` 內容，亦不可預先暫存或提交敏感設定。
*   **精準控制變更範圍**：AI 的代碼變更必須高度聚焦，嚴禁隨意重構不相干檔案，以保持 Git Commit Diff 的乾淨與易讀性。

---

## 🛠️ 技術棧規範一覽

| 技術棧 | 實體版本 / 限制 | 關鍵開發規範與相容性要求 |
| :--- | :--- | :--- |
| **Java** | JRE `1.8.0_211` (JDK 8) | 嚴禁使用 Java 9+ 語法特性（如 `var`、`List.of()`）。修改 `build.gradle` 後需提示執行 `./gradlew eclipse` 同步。 |
| **PHP** | PHP `8.1.2` | 遵循 **PSR-12**，可使用 PHP 8.1 特性，但嚴禁使用更高版本。PHP 依賴需經由 `composer` 管理。 |
| **MySQL** | MySQL `8.0.31` | 撰寫任何 SQL 查詢或 Schema Migration 需確保與 8.0.31 完全相容。 |
| **Node.js**| Node `v12.19.0` | 確保引入的新依賴套件與 Node v12 完全相容。 |
| **Vue** | Vue 2 / Vue 3 嚴格區分 | 嚴禁在 Vue 2 專案中直接寫 Vue 3 Composition API 語法，除非已明確配置相容套件。 |

---

## 📝 授權與貢獻規範

本專案遵循 Conventional Commits 規範（如 `feat:`, `fix:`, `chore:` 等前綴）進行 Commit 紀錄管理。
在提交任何修改前，請優先對齊並確認專案進度追蹤系統中的「RD專案管理」任務，確保變更與進度完美同步。
