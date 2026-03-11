# samzhu-agent-skills

[繁體中文](README.zh-TW.md)

Development workflow and creative tools skills for Claude Code.

## Installation

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

| Skill | Description |
|-------|-------------|
| **tdd** | Test-Driven Development with strict Red-Green-Refactor cycle |
| **bdd** | Behavior-Driven Development: Discovery → Formulation → Automation |
| **springboot-config-organizer** | Spring Boot configuration dual-layer profile design organizer |
| **research** | Research development topics and produce structured tutorial documents |
| **ui-craft** | Intentional UI design with craft quality, not defaults |

## Usage

### TDD

Trigger words: `TDD`, `test first`, `test-driven`, `Red-Green-Refactor`

The skill enforces the strict Red-Green-Refactor cycle: write a failing test first, implement minimal code to pass, then refactor.

### BDD

Trigger words: `BDD`, `Gherkin`, `scenario`, `feature file`, `acceptance criteria`, `user story`, `Example Mapping`

Covers three phases: Discovery (explore requirements with examples), Formulation (write Gherkin scenarios), and Automation (outside-in development).

### Spring Boot Config Organizer

Trigger words: `Spring Boot config`, `application.yml`, `profile`

Organizes Spring Boot configuration files using a dual-layer profile design pattern.

### Research

Trigger words: `research`, `tutorial`

Researches development topics using web search and produces structured, bilingual tutorial documents.

### UI Craft

Trigger words: `UI`, `design system`, `craft`, `shadcn`

Guides intentional UI design decisions with craft quality — every visual choice should be deliberate.

## Example

### `/springboot-config-organizer` — Organize Spring Boot Config

**Prompt:**

```
Use /springboot-config-organizer to organize the backend config files
```

**Output:**

The skill first analyzes issues in existing config files:

| Issue | Description |
|-------|-------------|
| Duplicate YAML keys | `application.yml` has two `aci:` blocks, the latter overrides the former |
| Single file too large | All configuration in one `application.yml` with no profile separation |
| No unified secret strategy | API keys scattered with `${ENV_VAR:}`, no secrets file management |
| Thin prod profile | `application-prod.yml` only overrides one system prompt |

Then suggests a new structure:

```
backend/
├── src/main/resources/                 (packaged into Docker Image)
│   ├── application.yaml                ← Base shared config
│   ├── application-local.yaml          ← Local infrastructure
│   └── application-gcp.yaml            ← GCP infrastructure
│
├── config/                             (external config, not packaged)
│   ├── application-dev.yaml            ← Dev environment (DEBUG logs, import secrets)
│   ├── application-prod.yaml           ← Production environment
│   └── application-secrets.properties.example  ← Secrets template
│
└── src/test/resources/
    └── application-test.yml            ← Unchanged
```

With unified property naming:

| Property | Purpose | Default |
|----------|---------|---------|
| `aci-oauth2-issuer-uri` | OIDC Provider Issuer URI | (empty) |

## License

MIT License — see [LICENSE](LICENSE) for details.
