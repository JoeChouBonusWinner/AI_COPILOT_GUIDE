# 跨工具 AI 開發通則與自動設定生成器 (Universal AI Copilot & Workspace Guide)

> 💡 **[META-INSTRUCTION / 超級元指令 - 優先度最高]**
> 當任何 AI 輔助開發工具（例如 Cursor, Claude Code, Cline, Roo Code, Gemini CLI, Antigravity CLI 等）讀取到本檔案時，**僅能針對當前正在使用的該款工具，自動在工作區根目錄中建立或更新其專屬的規則設定檔（嚴禁一次性產生或覆寫其他非當前使用工具的設定檔）**，並完整將本檔案下方的「開發通則與防禦原則」寫入其中：
> - 若確認當前使用者正在使用 **Cursor**，請僅生成/覆寫：**`.cursorrules`**
> - 若確認當前使用者正在使用 **Claude Code** (Anthropic CLI)，請僅生成/覆寫：**`CLAUDE.md`**
> - 若確認當前使用者正在使用 **Cline** 或 **Roo Code**，請僅生成/覆寫：**`.clinerules`** 或 **`.instructions.md`**
> - 若確認當前使用者正在使用 **Gemini CLI**，且 `GEMINI.md` 遺失，請僅生成：**`GEMINI.md`**
> - 若確認當前使用者正在使用 **Antigravity CLI / IDE**，請僅生成/覆寫：**`AGENTS.md`** 或 **`.agents/rules/workspace.md`**
> 
> ⚠️ **環境與語言檢查要求**：
> 1. **環境檢查**：在建立或更新上述檔案及後續執行任何系統指令前，**AI 必須先主動檢查或探測當前的主機作業系統環境（是 Windows 還是 Linux）**，以避免誤用不相容的 Shell 指令或路徑格式。
> 2. **語言偏好**：AI 在後續的所有回覆、文件生成及與使用者的對話中，**必須一律優先使用「繁體中文」 (Traditional Chinese / zh-TW)**。
> 3. **規則寫入**：在自動生成任何工具的專屬規則設定檔時，**必須將此「環境作業系統探測」與「優先使用繁體中文」規則一併寫入其中**，確保後續 AI 交互均能高度貼合使用者偏好並對環境保持敏感。
> 
> 建立或更新上述檔案時，請確保內容與本檔案完全相同（並包含此環境探測強化條款）。這能保證不論團隊使用哪一款 AI 開發工具，AI 的技術限制、套件管理規範、與測試提醒行為皆完全保持齊一。

---

## 📌 開發者使用指南 (User Quickstart Guide)

這份開發規範指南（如 `GEMINI.md` / `AI_COPILOT_GUIDE.md` / `AGENTS.md`）是一份「通用 AI 開發規範與自動引導書」。當您在全新的環境、分支，或是剛安裝新的 AI 工具（如 Cursor, Claude Code, Cline, Gemini CLI, Antigravity CLI 等）時，請依照以下步驟啟用本規範：

1. **首次引導與初始化 (Bootstrapping)**：
   - 請在您使用的 AI 聊天視窗中輸入以下指令，以觸發 AI 讀取本文件並自動生成對應的工具設定檔：
     > *"請詳細閱讀工作區下的 AI 規範引導文件（如 `GEMINI.md` 或 `AI_COPILOT_GUIDE.md`），並根據檔案最上方的「META-INSTRUCTION」為我目前所使用的 AI 工具（例如 Cursor / Claude Code / Cline / Roo Code / Gemini CLI / Antigravity 等）自動建立或更新其專屬的規則設定檔。在生成過程中，請務必先探測我目前的作業系統環境（Windows 還是 Linux），以確保規則檔內所包含的指令與路徑與我的環境完全適配。"*
2. **工具規範自動落地**：
   - AI 工具讀取此引導後，會自動在根目錄生成對應的規則檔案（如 `.cursorrules`、`CLAUDE.md`、`.clinerules`、`GEMINI.md`、`AGENTS.md` 等）。這能確保後續您在該工具中進行任何開發、提問或重構時，AI 都能無縫、精確地遵守專案的技術棧限制（例如 Java 8 限制、Vue 2 限制、環境相容性、i18n 規範等）。
3. **持續同步**：
   - 若未來本指南有任何更新，您只需再次要求 AI 「重新閱讀引導文件並同步更新工具規則」即可，簡單且高效。

---

## 📌 工作區 AI 開發通則與防禦原則 (Workspace Standards)

本工作區為多技術棧異質專案，所有 AI 代理人在進行程式碼編寫、重構、套件安裝及測試時，**必須絕對優先遵守**以下所列之規範。

---

### 1. 主要開發項目與架構概述
本工作區之主要開發項目明確劃分如下：

#### A. JAVA 專案 (基於 Java/Gradle)
- **獨立運行與服務專案**：
  - `bmw_login`：手機登入相關服務。
  - `bmw_official`：手機儲值服務。
  - `bw_login`：網頁登入相關服務。
  - `bw_official`：網頁儲值服務。
  - `bw_personal`：個人資料服務。
  - `bw_platform`：平台核心服務。
  - `bw_scratch`：刮刮樂相關服務。
  - `bw_batch`：批次任務與排程執行專案。
- **共用項目庫 (Library)**：
  - `bw_common`：**其他 Java 專案的共用項目庫 (Shared Library/Lib)**。修改此專案內的 API 或類別時，必須特別注意向下相容性，以防破壞其他依賴它的 Java 專案。

#### B. WEB 專案 (前端與 PHP 後端)
- `bw_apache`：主要的 Web 前端與服務，包含前端原始碼（如 `vue_src`、`svelte_src`）及相關 API/PHP 服務。
- `bw_backend`：後端管理系統與 API 服務，包含 PHP 後端（`backend_php`）與管理介面前端（`vue_src`）。

#### C. 唯讀保護區 與 開發工具隔離 (Read-Only & IDE Protection)
- **Eclipse 元數據隔離**：根目錄下的 `.metadata/` 目錄（包含其子目錄 `.plugins/`、`version.ini` 等）為 Eclipse 工作區的核心運行系統暫存。**AI 絕對不得讀取、修改、索引或搜尋其中的檔案**，避免破壞開發工具之穩定性。
- **未列管目錄**：所有未列在「JAVA 專案」與「WEB 專案」之目錄（例如 `TDY_*`、`cicd/`、`mcp_jenkins/`、`apache_tw/`、`i18n/`、`RemoteSystemsTempFiles` 等），皆預設為**唯讀保護區**。未經使用者明確指示，AI 絕對不得修改、刪除或新增唯讀保護區內的任何程式碼、組態設定或檔案。
- **Git 忽略規範**：嚴禁將 `.metadata/`、各 Java 專案產生的編譯輸出（如 `bin/`、`build/`）以及前端依賴庫（如 `node_modules/`）進行 Git 暫存或提交。
- **非 Git 環境下全域忽略與 AI 沙盒規範 (`.ignore` 檔案)**：
  - 由於本工作區非處於 Git 版本控制，為防止各 AI 工具（如 Gemini CLI、Antigravity CLI、Cursor、Claude Code 等）盲目掃描龐大且無效的暫存與編譯檔案而耗盡 Token，工作區根目錄已建立 **`.ignore`** 檔案。
  - 各 AI 工具在啟動、建立全域索引或搜尋時，**必須自動讀取並嚴格遵守根目錄下的 `.ignore` 忽略配置**。
  - ⚠️ **沙盒保留原則**：工作區內建的 **`ai_temp/`** 與 **`ai_download/`** 為 AI 專屬沙盒運作與下載目錄，**嚴禁將此兩目錄列入 `.ignore` 忽略清單**。這能確保 AI 代理人在下載、暫存與分析檔案時，能保有 100% 完整的檔案檢索、讀寫與 `glob` 搜尋視野。

---

### 2. 套件與依賴管理規範 (關鍵防錯)

為了避免混用不同的套件管理器導致專案鎖定檔 (Lockfile) 衝突，AI 在安裝或更新依賴時必須遵循以下規則：

#### 前端專案 (NodeJS)
- **環境與套件管理工具規範**：
  - 目前實體前端運行的 Node.js 版本為 **`v12.19.0`**。AI 在編寫前端程式碼、引進新依賴套件或調整建置配置時，必須確保與此 Node.js 版本完全相容。嚴禁引入需要高版本 Node 運行時的套件。
  - **`bw_apache` 專案**：**一律以 `npm` 為主** 進行套件與依賴管理。執行安裝或新增套件時，應使用 `npm install`。
- **清除重複鎖定檔**：若在同一目錄下發現多個不同的鎖定檔（例如同時存在 `package-lock.json`、`pnpm-lock.yaml` 與 `yarn.lock`），在未獲得使用者明確指示前，**不得隨意刪除**，但必須使用該專案正確指定的工具（如 `bw_apache` 統一使用 `npm`）。

#### PHP 專案
- **Composer 規範**：PHP 依賴必須透過 `composer` 進行管理，嚴禁直接修改 `vendor/` 目錄下的內容。

#### Java 專案
- **Gradle 依賴**：所有 Gradle 依賴的修改必須寫入專案對應的 `build.gradle`（或特定版本的 `build7.gradle`），並確保與全域 `bw_common` 的版本相容。

---

### 3. Java & Gradle 開發通則

- **Gradle Wrapper 優先**：執行編譯、測試、打包命令時，必須使用專案目錄下的 `./gradlew` (Linux/Mac) 或 `gradlew.bat` (Windows)，不得直接呼叫全域的 `gradle` 命令。
- **Java 版本與語意限制**：
  - 目前專案使用的實體 JRE 版本為 **`1.8.0_211`**。
  - AI 在生成、重構或修改 Java 程式碼時，**必須嚴格限制僅能使用 Java 8 (JDK 1.8) 的相容語法**。
  - 嚴禁使用 JDK 9 以上的高版本語法特性（例如：`var` 本地變數宣告、`switch expressions`、`List.of()` 唯讀列表等），以防止在打包與執行時發生相容性或編譯失敗。
- **Build 與 Eclipse 設定同步**：
  - 本工作區中部分 Java 專案使用 Eclipse 作為開發環境（存在 `.classpath`、`.project` 與 `.settings/` 等目錄）。
  - 當 AI 修改了任何專案的 `build.gradle`（例如新增、刪除或調整 Gradle 依賴設定）後，**必須直接在對應專案目錄下執行或提示使用者執行 `./gradlew eclipse` 指令**，以直接更新並同步最新的 Eclipse 專案設定檔。請**不要**使用、呼叫或指引使用者去點擊根目錄的批次檔 (bat) 進行更新。
- **建置配置版本差異**：
  - 本工作區中部分專案（如 `bw_batch`）同時存在 `build.gradle` 與 `build7.gradle`。AI 在新增依賴或調整編譯配置時，**必須先確認使用者目前編譯所使用的 Gradle 設定檔**，避免修改錯誤的版本。
- **測試與驗證**：
  - 任何 Java 程式碼修改完成後，均應嘗試在專案目錄下執行 `./gradlew test` 以進行行為驗證。如果是修改 `bw_common`，需一併驗證其他依賴專案的編譯正確性。

---

### 4. 前端與 PHP 開發通則

#### PHP 與資料庫 (後端)
- **環境版本與 SQL 安全限制**：
  - 目前 Apache 伺服器運行的實體 PHP 版本為 **`PHP 8.1.2`**。AI 在生成、重構或修改 PHP 程式碼時，必須確保與此版本相容（可使用 PHP 8.1 特性，如 read-only properties、match 表達式等，但嚴禁使用高於此版本的語法與 API）。
  - 目前使用的實體 MySQL 伺服器版本為 **`8.0.31`**。AI 撰寫或修改任何 SQL 查詢或 Schema Migration 時，必須確保與 MySQL 8.0.31 完全相容。
  - 🛑 **毀滅性 SQL 操作限制**：嚴禁 AI 執行或寫入破壞性 SQL 操作（如 `DROP DATABASE`、`TRUNCATE TABLE` 或未指定 `WHERE` 條件的 `DELETE`/`UPDATE`），除非獲得使用者明確書面授權。
- **程式碼風格與架構**：
  - 遵循 **PSR-12** 規範。
  - 嚴禁撰寫未經驗證的 Raw SQL；一律遵循專案現有的 ORM (如 PHP 既有的資料庫存取層模式) 或是 Java 的 DAO/MyBatis/Hibernate 模式，並確保連線與查詢正確釋放。
- **路由與控制器**：新增 API 或功能時，應遵循專案現有的目錄結構（例如 `api/` 或 `backend_php/`），並在既有的路由註冊機制中進行註冊。

#### 前端 (Vue & Svelte)
- **Vue 版本相容性**：
  - 必須嚴格區分專案是 Vue 2 還是 Vue 3。
  - **嚴禁**在 Vue 2 的專案（如舊版 `vue_src`）中寫入 Vue 3 Composition API 的語法，除非該專案已明確配置 `@vue/composition-api`。
- **多國語系 (i18n) 開發優先**：
  - 於 WEB 相關專案（`bw_apache`、`bw_backend` 等）中進行功能開發時，**一律優先使用多國語系 (i18n) 進行文字處理**。
  - 嚴禁將任何 UI 顯示文字、按鈕文字或提示文字直接硬編碼（Hardcode）在 HTML、Vue, Svelte 或 PHP 視圖程式碼中。必須使用專案現有的多國語系資源或鍵值 (Key) 配置進行替換。
- **本地測試提醒**：在修改前端（Vue/Svelte/JS）原始碼完成後， AI **必須主動詢問使用者**：「是否需要協助執行 npm 或 Vue 相關指令來啟動本地測試環境進行畫面驗證？」除非使用者明確要求，否則不應自動執行打包 (Build)。

---

### 5. AI 協作防禦性原則

- **環境作業系統探測與語意規範 (OS Detection & Language Rules)**：
  - 遵循檔案頂部 [META-INSTRUCTION] 之規定：執行 Shell 指令前必須探測 Host OS（Windows/Linux）並自動調整路徑與分隔符；對話、註解與檔案寫入一律強制使用**繁體中文 (zh-TW)**。
- **Token 與上下文消耗保護 (Context Window Safeguard)**：
  - AI 在檢索或閱讀原始碼、Log 與設定檔時，**應善用行數限制與精準關鍵字搜尋（如 `grep`、`view_file` 行數範圍設定）**，避免全檔大爆發一次性輸出大量無關內文而耗盡 Token 與上下文空間。
- **跨平台換行符號 (Line Endings) 規範**：
  - 由於開發環境在 **Windows (win32)** 上執行，但測試與正式環境多在 **Linux** 運行。
  - **所有 Shell 腳本（`.sh` 檔案）與伺服器/容器組態設定檔**，在寫入與修改時，**一律強制使用 `LF` (Unix) 換行格式存檔**，嚴禁使用 `CRLF`（以防止在 Linux 下報錯如 `^M / bad interpreter`）。
  - 其它程式碼檔案（Java, PHP, JS, Vue, Svelte）在存檔時，應保持與該專案既有檔案相同的換行格式設定（預設推薦採用 UTF-8 無 BOM 且 LF 格式）。
- **禁止硬編碼網路與連接位址 (No IP/Host Hardcoding)**：
  - 嚴禁在程式碼中寫死（Hardcode）任何特定的實體內網 IP 位址（如 `172.25.*`、`192.168.*`）、外網 IP 或本機連接位址（如 `127.0.0.1`）。
  - 所有的資料庫、Redis、連線埠 (Port) 或外部 API 網址，**一律必須引用對應專案的環境變數、配置、或既有的設定檔**，確保本機開發與測試/正式環境能無縫、安全地切換。
- **檔案編碼格式 (File Encoding)**：
  - 本工作區之所有原始碼與設定檔（包含 Java、PHP、JS、Vue、Svelte、Markdown、XML、Gradle 等）一律強制使用 **`UTF-8`** 編碼保存。
  - 嚴禁使用 Windows 系統預設的 `MS950/Big5` 編碼，且**不帶有 UTF-8 BOM 檔頭**，以防止在建置、跨平台運行時發生中文亂碼或編譯解析錯誤。
- **機密、金鑰與設定檔防護 (Secrets & Configuration Protection)**：
  - **真實憑證防漏**：AI 絕對不得在終端機輸出、Log、測試報告、程式碼註解、或任何 Git 提交中暴露、儲存或提交任何真實的密碼、API 金鑰、私鑰、資料庫連線憑證或 `.env` 檔案內容。
  - **嚴禁 Stage/Commit 敏感設定檔**：絕不可將包含真實金鑰、密碼或本機特定路徑的設定檔（例如 `proxy.js`、`.env`）進行 Git 暫存 (Stage) 或提交 (Commit)。
  - **同步更新設定範本**：若為專案新增、修改或調整了設定項目，必須主動同步更新其範本檔（如 `proxy.js.example`），以利團隊成員參考與無縫切換。
- **精準控制變更範圍與破壞性 Git 操作限制 (Surgical Edits & Git Safety)**：
  - **局部、精準修改 (Surgical Edits)**：AI 必須嚴格圍繞任務進行針對性修改，優先使用精準替換工具，嚴禁隨意「順便」重構無關代碼或調整無關變更，保持既有縮排與美觀。
  - **不要預先 Stage/Commit**：除非使用者有明確指令，否則 AI 絕對不能主動執行 `git add` 或 `git commit`。
  - 🛑 **破壞性 Git 操作限制**：嚴禁在未獲得使用者明確同意前，執行任何具毀滅性或不可逆的 Git 操作（如 `git checkout .` 強制覆蓋全區變更、`git reset --hard`、`git clean -fd` 等）。
- **測試案例 (Test Case) 提醒**：
  - 在完成修改與重構後，AI **必須主動詢問**：「是否需要為本次修改新增或更新相對應的 Test Case？」
  - 設計測試時，**絕對不得直接修改 Source Code**；若不修改 Source Code 即無法撰寫 Test Case，AI 必須在實作前詳細說明並向使用者請示。
- **Git Commit 訊息與專案管理關聯**：
  - 產生 Commit 訊息時應遵循 Conventional Commits 規範。
  - 產生 Commit 前，**必須主動提醒使用者先確認「RD專案管理」內容**，優先使用對應項目名稱或任務標題作為 commit 主體。
- **優先使用本地排版與檢查工具 (Linter & Formatter Priority)**：
  - 修改前後若專案有 ESLint、Prettier、php-cs-fixer、Gradlew fmt 等工具，應優先使用或提示執行，確保風格一致。
- **自動產生的臨時/一次性檔案生命週期管理與臨時目錄規範 (Temporary & One-off Files Management)**：
  - **自動清理原則**：執行一次性任務、比對工具或測試腳本產生的臨時檔案，**任務結束後 AI 必須主動刪除**。若具保留價值，需主動詢問使用者。
  - **統一在 `ai_temp/` 存取**：所有臨時/一次性檔案一律統一存放在 `C:\workspace\ai_temp`，嚴禁散落於其他子目錄。
  - **下載檔案管理在 `ai_download/`**：透過 Rclone、Curl 等下載的外部檔案，一律統一存放於 `C:\workspace\ai_download`。
  - **Google Drive 下載**：強制優先使用 `rclone`，且下發指令前必須要求提供完整雲端路徑，嚴禁盲目猜測。
- **程式碼適度註解與自文件化規範 (Code Commenting & Self-Documentation Standard)**：
  - 必須在複雜邏輯、業務分支、非直觀演算法或跨系統通訊處留下清晰中文註解。說明「設計意圖（Why）」與「業務背景」，避免冗餘說明語句。

---

### 6. 跨專案 API 介面合約安全 (Java ↔ PHP)

- **API 變更強制警告**：
  - 本工作區之 Java 後端服務（如 `bw_platform`、`bw_login` 等）與 PHP 後端 WEB（如 `bw_backend`、`bw_apache` 等）存在相互呼叫通訊。
  - 修改或新增跨專案通訊 API 欄位、JSON 格式、URL 路徑或 HTTP 狀態碼時，**必須強烈提醒並提示使用者確認『跨專案相容性』**。
  - 嚴禁未經授權擅自重構或變更既存 API 欄位型態與鍵值命名規範（如底線與駝峰轉換），防止通訊中斷。

---

### 7. 自訂 Skill 模組化與架構擴充指引 (Skills Framework Integration)

- **SOP 模組化原則**：
  - 對於高度重複、跨專案或複雜的開發 SOP（例如 Eclipse 工作區設定同步、Gradle 發布與建置排程、多國語系 i18n 鍵值對照檢查、資料庫 Migration 檢查腳本等），**建議優先將其抽離並寫入專案的自訂 Skill 模組（存放於 `.agents/skills/<skill_name>/SKILL.md`）**。
- **保持主規則檔精簡**：
  - 主規則檔（`GEMINI.md` / `AGENTS.md` / `AI_COPILOT_GUIDE.md`）應專注於**核心原則與安全邊界防禦**；專門技能與技術腳本細節則交由 Skill 模組進行動態加載與維護。
