# samzhu-agent-skills

Development workflow and creative tools skills for Claude Code.

提供開發工作流程與創意工具的 Claude Code Skills 集合。

## Installation / 安裝

Add the marketplace:

```bash
claude plugin marketplace add samzhu/agent-skills
```

Install individual plugins:

```bash
claude plugin install tdd@samzhu-agent-skills
claude plugin install bdd@samzhu-agent-skills
claude plugin install springboot-config-organizer@samzhu-agent-skills
claude plugin install research@samzhu-agent-skills
claude plugin install ui-craft@samzhu-agent-skills
```

## Skills

| Skill | Description | 說明 |
|-------|-------------|------|
| **tdd** | Test-Driven Development with strict Red-Green-Refactor cycle | 嚴格的紅-綠-重構 TDD 工作流程 |
| **bdd** | Behavior-Driven Development: Discovery → Formulation → Automation | 行為驅動開發：探索 → 撰寫場景 → 自動化 |
| **springboot-config-organizer** | Spring Boot configuration dual-layer profile design organizer | Spring Boot 設定檔雙層 profile 設計組織器 |
| **research** | Research development topics and produce structured tutorial documents | 研究開發主題並產出結構化教學文件 |
| **ui-craft** | Intentional UI design with craft quality, not defaults | 刻意的 UI 設計，追求工藝品質而非預設值 |

## Usage / 使用方式

### TDD

Trigger words: `TDD`, `test first`, `test-driven`, `Red-Green-Refactor`, `寫功能`, `修 bug`, `單元測試`

The skill enforces the strict Red-Green-Refactor cycle: write a failing test first, implement minimal code to pass, then refactor.

### BDD

Trigger words: `BDD`, `Gherkin`, `scenario`, `feature file`, `acceptance criteria`, `user story`, `Example Mapping`, `需求`, `驗收`, `使用者故事`

Covers three phases: Discovery (explore requirements with examples), Formulation (write Gherkin scenarios), and Automation (outside-in development).

### Spring Boot Config Organizer

Trigger words: `Spring Boot config`, `application.yml`, `profile`, `設定檔`, `組態`

Organizes Spring Boot configuration files using a dual-layer profile design pattern.

### Research

Trigger words: `research`, `研究`, `調研`, `教學文件`, `tutorial`

Researches development topics using web search and produces structured, bilingual tutorial documents.

### UI Craft

Trigger words: `UI`, `介面設計`, `design system`, `craft`, `shadcn`

Guides intentional UI design decisions with craft quality — every visual choice should be deliberate.

## License

MIT License — see [LICENSE](LICENSE) for details.
