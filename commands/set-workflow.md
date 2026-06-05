---
name: set-workflow
description: Configure the dev-flow development pipeline. Manage auto-loaded skills, product team personas, pipeline gates, and strict mode. Use for "configure dev-flow", "add skill to pipeline", "manage product team", "toggle pipeline gates", "set up my workflow".
argument-hint: "[add|remove|list|product|strict|reset|status]"
---

# /set-workflow — Pipeline Configuration

Manage your set-workflow pipeline configuration stored at `~/.claude/set-workflow.local.md`.

## Usage

### `/set-workflow` (no args) — Status

Show current pipeline configuration and status: active auto-load skills, enabled gates, product team composition, strict mode state.

### `/set-workflow add <skill-name>`

Add a skill to the auto-load list. Accepts standalone skills (`code-review`) or namespaced plugin skills (`plugin:skill`).

Example: `/set-workflow add code-review`

### `/set-workflow remove <skill-name>`

Remove a skill from the auto-load list.

Example: `/set-workflow remove simplify`

### `/set-workflow list`

Display all currently configured auto-load skills with their source.

### `/set-workflow product` — Product Team Management

Manage the multi-persona product discussion team.

Sub-commands:

| Command | Description |
|---------|-------------|
| `/set-workflow product list` | List current product team members with their perspectives |
| `/set-workflow product add <name>` | Add a new product persona. You will be prompted for their perspective/label |
| `/set-workflow product remove <name>` | Remove a product persona |
| `/set-workflow product preset default` | Reset to default 5-role team (Pragmatic, User Advocate, Innovator, Business, Tech-Aware) |
| `/set-workflow product preset lean` | Use lean 2-role team (Pragmatic + User Advocate) |

### `/set-workflow strict on|off`

Toggle strict mode. When ON, any pipeline phase that fails blocks progression to the next phase. When OFF, phases that fail issue warnings but allow continuation.

### `/set-workflow reset`

Reset all configuration to factory defaults. Prompts for confirmation before executing.

## Configuration File

All settings are persisted to `~/.claude/set-workflow.local.md` (user-global). This file uses YAML frontmatter for structured data and Markdown body for freeform notes. It is safe to edit manually if preferred.

## Examples

```bash
# Quick setup: add code review to pipeline
/set-workflow add code-review

# Build a product team
/set-workflow product preset default
/set-workflow product add security-expert

# Lock it down
/set-workflow strict on

# See what's active
/set-workflow
```
