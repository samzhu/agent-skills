# samzhu-agent-skills

[English](README.md)

提供開發工作流程與創意工具的 Claude Code Skills 集合。

## 安裝

加入 marketplace：

```bash
claude plugin marketplace add samzhu/agent-skills
```

安裝個別 skill：

```bash
claude plugin install tdd@samzhu-agent-skills
claude plugin install bdd@samzhu-agent-skills
claude plugin install springboot-config-organizer@samzhu-agent-skills
claude plugin install research@samzhu-agent-skills
claude plugin install ui-craft@samzhu-agent-skills
claude plugin install depx@samzhu-agent-skills
claude plugin install handover@samzhu-agent-skills
claude plugin install takeover@samzhu-agent-skills
claude plugin install retro@samzhu-agent-skills
```

## Skills 清單

| Skill | 說明 |
|-------|------|
| **tdd** | 嚴格的紅-綠-重構 TDD 工作流程 |
| **bdd** | 行為驅動開發：探索 → 撰寫場景 → 自動化 |
| **springboot-config-organizer** | Spring Boot 設定檔雙層 Profile 設計組織器 |
| **research** | 研究開發主題並產出結構化教學文件 |
| **ui-craft** | 刻意的 UI 設計，追求工藝品質而非預設值 |
| **depx** | 探索 JVM 依賴套件原始碼，索引並反編譯 JAR 檔案 |
| **handover** | 將 session 上下文儲存為結構化筆記，讓任何 agent 或人類接手 |
| **takeover** | 讀取交班筆記、歸檔、呈報摘要，無縫接續工作 |
| **retro** | 基於證據的回顧，產出可重複使用的觸發-行動清單 |

## 使用方式

### TDD

觸發詞：`TDD`、`test first`、`test-driven`、`Red-Green-Refactor`、`寫功能`、`修 bug`、`單元測試`

此 skill 強制執行嚴格的紅-綠-重構循環：先寫一個會失敗的測試、寫最少的程式碼讓測試通過、接著重構。

### BDD

觸發詞：`BDD`、`Gherkin`、`scenario`、`feature file`、`acceptance criteria`、`user story`、`Example Mapping`、`需求`、`驗收`、`使用者故事`

涵蓋三個階段：探索（用範例釐清需求）、撰寫（寫 Gherkin 場景）、自動化（由外而內開發）。

### Spring Boot Config Organizer

觸發詞：`Spring Boot config`、`application.yml`、`profile`、`設定檔`、`組態`

使用雙層 Profile 設計模式整理 Spring Boot 設定檔。

### Research

觸發詞：`research`、`研究`、`調研`、`教學文件`、`tutorial`

透過網路搜尋研究開發主題，產出結構化的雙語教學文件。

### UI Craft

觸發詞：`UI`、`介面設計`、`design system`、`craft`、`shadcn`

引導刻意的 UI 設計決策，追求工藝品質 — 每個視覺選擇都應該是有意圖的。

### Depx（依賴套件探索器）

觸發詞：`dependency source`、`decompile`、`JAR`、`inspect class`、`library internals`、`查看依賴`、`反編譯`

**為什麼需要這個 skill：** 在 JVM 專案中使用 Claude Code 時，查看第三方依賴的原始碼是很常見的需求 — 確認類別簽章、理解函式庫內部運作、或找到正確的 API 呼叫方式。沒有這個 skill 的話，Claude Code 會反覆執行 `find ~/.gradle/caches` 和 `javap` 指令，例如：

```
JAR=$(find ~/.gradle/caches -name "some-lib-0.10.0.jar" 2>/dev/null | head -1) \
  && javap -p -classpath "$JAR" com.example.SomeClass
```

每一條指令都會觸發權限確認提示（「Command contains `$()` command substitution — Do you want to proceed?」），讓一次簡單的類別查詢變成反覆按確認的苦差事。單次調查可能就需要 5～10 次核准。

Depx 的解法是一次性建立本地 `.depx/` 索引（manifest 檔案可透過 Grep 工具搜尋，不需要權限提示），並按需反編譯 JAR。初次設定完成後，大部分查詢**完全不需要 Bash 指令，也不會跳出任何權限提示**。

索引 JVM 依賴套件的 JAR 檔案，可搜尋類別簽章或按需反編譯完整原始碼。支援 Gradle 與 Maven 專案。

### Handover & Takeover（交班與接班）

<p align="center">
  <img src="images/handover-takeover.png" alt="Handover & Takeover 流程圖" width="600" />
</p>

#### Handover（交班）

觸發詞：`handover`、`交班`、`換手`、`shift change`、`save progress`、`wrap up session`、`pass the baton`、`先到這裡`、`存檔`

**為什麼需要這個 skill：** Claude Code 的 session 有 context 上限，對話越長越容易遺失早期的細節。當你要結束工作、切換到另一個 session、或 context 快滿的時候，需要一個方法把進度「存檔」。

Handover 會自動掃描整段對話歷史，產出一份結構化的 `.claude/handovers/HANDOVER.md`，分成兩層：

| 層 | 內容 | 誰能讀 |
|----|------|--------|
| **Layer 1 — 通用摘要** | 主題、狀態、完成項目、決策（含被否決的替代方案）、blockers、下一步、踩坑紀錄 | 任何 agent 或人類 |
| **Layer 2 — 環境細節** | git branch、未提交的檔案、最近 commit、測試狀態、相關檔案 | 同一台機器/同一個 repo |

根據 session 類型（development / debug / research / refactor / planning）自動調整重點深度。例如 debug session 會特別詳細記錄「試過什麼、為什麼失敗」，避免下個人重踩同樣的坑。

#### Takeover（接班）

觸發詞：`takeover`、`接班`、`pick up where we left off`、`resume handover`、`continue from last session`、`read handover`

**為什麼需要這個 skill：** 開啟新 session 時，你面對的是一個完全沒有上下文的 agent。沒有 takeover 的話，你得自己口述「上次做到哪裡」，既費時又容易漏掉細節。

Takeover 會自動讀取 `/handover` 留下的筆記，然後：

1. 呈報結構化摘要（做了什麼、關鍵決策、blockers、行動計畫）
2. 歸檔已消費的筆記到 `.claude/handovers/archive/`（防止重複讀取）
3. 等你確認後才開始動工

這對 skill 是 **agent-agnostic** 的 — Layer 1 的格式設計讓 Claude、Gemini、Copilot 或人類都能讀懂。

### Retro（回顧）

<p align="center">
  <img src="images/retro.png" alt="Retro 覆盤流程圖" width="600" />
</p>

觸發詞：`retro`、`retrospective`、`lessons learned`、`what went wrong`、`post-mortem`、`review this session`、`what could be improved`

**為什麼需要這個 skill：** 完成一個任務後，最有價值的不是「做完了」，而是「下次怎麼做得更好」。但一般的回顧往往流於空泛的反省（「我應該更仔細」），無法轉化成具體行動。

Retro 用 7 步驟的結構化協議產出基於證據的回顧：

| 步驟 | 內容 |
|------|------|
| **1. 轉折點帳本** | 列出所有方向改變的時刻，標記是使用者觸發 (A) 還是自行發現 (B)，算出 A:B 比例 |
| **2. 5 Whys** | 對每個 (A) 轉折點追問五次「為什麼」，每個答案必須引用具體證據 |
| **3. 根因** | 從預設清單中選出唯一一個主要根因 |
| **4. 反事實** | 誠實回答：如果使用者沒有介入，會停在哪裡、給出什麼錯誤答案 |
| **5. 觸發-行動清單** | 主要交付物 — 格式：When {觸發}, before {階段}, must {行動} |
| **5.5 人類回饋** | 暫停等使用者確認或修正（阻斷步驟） |
| **6. 持久化** | 將清單存成 skill 或寫入 CLAUDE.md，未來 session 自動觸發 |
| **7. 一句話結論** | 本次最貴的錯誤是什麼、花了幾輪對話 |

最終產出的觸發-行動清單是 **可攜式的** — 用英文撰寫、不含專案特定路徑，可跨專案重複使用。

## 範例

### `/springboot-config-organizer` — 整理 Spring Boot 設定檔

**提示：**

```
使用 /springboot-config-organizer 整理 backend 的設定檔
```

**輸出：**

Skill 會先分析現有設定檔的問題：

| 問題 | 說明 |
|------|------|
| YAML 重複 key | `application.yml` 有兩個 `aci:` 區塊，後者覆蓋前者，`aci.models` 可能沒生效 |
| 單檔過大 | 所有配置集中在 `application.yml`，無 profile 分層 |
| 機敏值無統一策略 | API keys 用 `${ENV_VAR:}` 分散在各處，沒有 secrets 檔案管理 |
| prod profile 過於單薄 | `application-prod.yml` 只覆蓋一個 system prompt |

然後建議新架構：

```
backend/
├── src/main/resources/                 (打包進 Docker Image)
│   ├── application.yaml                ← 基礎共用（合併 aci: 區塊、統一屬性名稱）
│   ├── application-local.yaml          ← 本地基礎設施
│   └── application-gcp.yaml            ← GCP 基礎設施
│
├── config/                             (外部配置，不打包)
│   ├── application-dev.yaml            ← 開發環境行為（DEBUG 日誌、import secrets）
│   ├── application-prod.yaml           ← 正式環境行為
│   └── application-secrets.properties.example  ← 機敏值範例檔
│
└── src/test/resources/
    └── application-test.yml            ← 不變
```

並統一屬性命名：

| 屬性名稱 | 用途 | 預設值 |
|----------|------|--------|
| `aci-oauth2-issuer-uri` | OIDC Provider Issuer URI | (空) |

## 授權條款

MIT License — 詳見 [LICENSE](LICENSE)。
