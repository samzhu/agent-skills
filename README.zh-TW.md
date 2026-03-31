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
