---
# Set-Workflow Pipeline Configuration
# Copy this file to ~/.claude/set-workflow.local.md (user home directory)
# Then customize the fields below to control your pipeline.
# Manage via /set-workflow command or edit manually.

# Skills to auto-load at Claude Code startup (all projects)
auto_load_skills: []
# Examples:
#   - code-review
#   - simplify
#   - andrej-karpathy-skills:karpathy-guidelines

# Product team — personas for Phase 1 multi-perspective discussion
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

# Strict mode — when true, phase failures block progression
strict_mode: false
---

# Set-Workflow Pipeline Configuration

## What This File Does

This configuration governs the set-workflow six-phase development pipeline:
1. **产品 (Product)** — Multi-persona discussion → PRD
2. **研发 (Tech Lead)** — Task decomposition
3. **开发 (Developer)** — Implementation
4. **测试 (QA)** — Testing & test report
5. **评审 (Review)** — Review board decision
6. **合入 (Merge)** — Commit or rework

## Quick Start

1. Copy this file to `~/.claude/set-workflow.local.md`
2. Customize the YAML frontmatter above
3. Start using Claude Code — the pipeline activates automatically
4. Use `/set-workflow` to manage settings interactively

## Interactive Commands

```bash
/set-workflow                          # View current pipeline status
/set-workflow add code-review          # Add a skill to auto-load
/set-workflow remove simplify          # Remove a skill
/set-workflow list                     # List auto-loaded skills
/set-workflow product list             # View product team
/set-workflow product add <name>       # Add a product persona
/set-workflow product remove <name>    # Remove a product persona
/set-workflow product preset default   # Reset to default 5-role team
/set-workflow product preset lean      # Use lean 2-role team
/set-workflow strict on                # Enable strict mode
/set-workflow strict off               # Disable strict mode
/set-workflow reset                    # Reset to factory defaults
```
