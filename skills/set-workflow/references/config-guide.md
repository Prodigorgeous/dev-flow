# Set-Workflow Configuration Guide

Complete reference for configuring the set-workflow pipeline.

---

## Configuration File

**Location**: `~/.claude/set-workflow.local.md` (user-global, applies to all projects)
**Format**: YAML frontmatter inside a Markdown file
**Management**: Via `/set-workflow` command or manual editing
**Required**: No — if absent, all defaults apply

The file is user-global (in your home directory `.claude/`), meaning one configuration applies to every project you work on. This is by design: the pipeline is your personal development workflow standard, not a per-project setting.

---

## Full Configuration Schema

```yaml
---
# Auto-load skills — invoked at startup in every session
auto_load_skills: []

# Product team — multi-persona discussion team for Phase 1
product_team:
  - name: pragmatic
    label: 务实派
    perspective: "Minimize complexity and cost. Ship the simplest thing that works. Respect deadlines and existing constraints."
  - name: user-advocate
    label: 用户派
    perspective: "Champion user experience above all. Users hate friction. Every extra step loses 20% of users. Make it intuitive."
  - name: innovator
    label: 创新派
    perspective: "Push boundaries. What's the differentiator? What would make this truly impressive? Think 2 years ahead."
  - name: business
    label: 商业派
    perspective: "Focus on market value, conversion rates, retention, and business metrics. Does this move the needle?"
  - name: tech-aware
    label: 技术派
    perspective: "Evaluate technical feasibility, system compatibility, scalability, security, and architectural fit."

# Strict mode — halt on phase failure vs. warn
strict_mode: false
---
```

---

## Field Reference

### `auto_load_skills`

Skills to automatically invoke at the start of every Claude Code session.

**Type**: `string[]`
**Default**: `[]`
**Example**:
```yaml
auto_load_skills:
  - code-review
  - simplify
  - andrej-karpathy-skills:karpathy-guidelines
```

**Guidance**:
- Start empty. Add skills as you discover which ones you always want active.
- Each auto-loaded skill consumes context tokens. Keep the list lean.
- Behavioral guideline skills (`karpathy-guidelines`, `simplify`) have the best ROI.
- Task-specific skills (database helpers, API generators) should be invoked manually, not auto-loaded.

---

### `product_team`

Defines the personas that participate in the Phase 1 product team discussion.

**Type**: Array of persona objects
**Default**: 5-role team (pragmatic, user-advocate, innovator, business, tech-aware)

**Persona object structure**:
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | Yes | Unique identifier (kebab-case) |
| `label` | string | Yes | Display name shown in discussion |
| `perspective` | string | Yes | The persona's viewpoint and decision criteria |

**Adding a persona**:
```bash
/set-workflow product add security-expert
```
You will be prompted for the label and perspective.

**Manually**:
```yaml
product_team:
  - name: security-expert
    label: 安全专家
    perspective: "Identify security vulnerabilities, attack vectors, and data privacy risks. Prefer secure-by-default designs."
```

**Removing a persona**:
```bash
/set-workflow product remove security-expert
```

**Preset teams**:
- `default` — 5 roles: pragmatic, user-advocate, innovator, business, tech-aware
- `lean` — 2 roles: pragmatic, user-advocate

**Guidance**:
- 3-5 personas provide good discussion diversity without excessive length
- Give each persona a genuinely distinct perspective — two personas with the same viewpoint waste tokens
- The discussion order follows the array order — put the most important perspective first
- All personas contribute to every Phase 1 discussion regardless of the topic

---

### `strict_mode`

Controls whether pipeline phase failures block progression.

**Type**: `boolean`
**Default**: `false`

**When `false` (default)**:
- Phase gate failures produce warnings
- User can choose to proceed despite warnings
- Appropriate for most workflows

**When `true`**:
- Phase gate failures block progression to the next phase
- The failing phase must be re-executed
- Appropriate for regulated environments, high-stakes projects, or onboarding

**Toggle**:
```bash
/set-workflow strict on
/set-workflow strict off
```

---

## Configuration File Location

The file `~/.claude/set-workflow.local.md` is stored in your home directory:

| Platform | Path |
|----------|------|
| macOS / Linux | `~/.claude/set-workflow.local.md` |
| Windows | `C:\Users\<username>\.claude\set-workflow.local.md` |

This is user-global scope — settings apply to every project. If you want project-specific overrides in the future, create `.claude/set-workflow.local.md` in the project root with only the fields you want to override.

---

## Example Configurations

### Strict Production Workflow

```yaml
---
auto_load_skills:
  - code-review
  - simplify
  - andrej-karpathy-skills:karpathy-guidelines

product_team:
  - name: pragmatic
    label: 务实派
    perspective: "Ship fast, minimize complexity."
  - name: user-advocate
    label: 用户派
    perspective: "UX is everything. Remove friction."
  - name: tech-aware
    label: 技术派
    perspective: "Feasibility, security, compatibility first."
  - name: security-expert
    label: 安全专家
    perspective: "Find vulnerabilities. Secure by default."

strict_mode: true
---
```

### Rapid Prototyping

```yaml
---
auto_load_skills: []

product_team:
  - name: pragmatic
    label: 务实派
    perspective: "Ship the simplest thing. We can iterate later."
  - name: user-advocate
    label: 用户派
    perspective: "Does this solve the user's actual problem?"

strict_mode: false
---
```

### Solo Developer with Code Review

```yaml
---
auto_load_skills:
  - code-review

product_team:
  - name: pragmatic
    label: 务实派
    perspective: "Keep it simple. Ship it."
  - name: tech-aware
    label: 技术派
    perspective: "Clean architecture. Don't create tech debt."

strict_mode: false
---
```

---

## Resetting Configuration

To restore factory defaults:

```bash
/set-workflow reset
```

This removes `~/.claude/set-workflow.local.md` and falls back to all default values. Prompts for confirmation before executing.
