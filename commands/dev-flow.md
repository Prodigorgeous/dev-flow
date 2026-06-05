---
name: dev-flow
description: Configure the dev-flow development pipeline. Manage auto-loaded skills, product team personas, pipeline gates, and strict mode. Use for "configure dev-flow", "add skill to pipeline", "manage product team", "toggle pipeline gates", "set up my workflow".
argument-hint: "[add|remove|list|product|strict|reset|status]"
---

# /dev-flow — Pipeline Configuration

Manage your dev-flow pipeline configuration stored at `~/.claude/dev-flow.local.md`.

## Usage

### `/dev-flow` (no args) — Status

Show current pipeline configuration and status: active auto-load skills, enabled gates, product team composition, strict mode state.

### `/dev-flow add <skill-name>`

Add a skill to the auto-load list. Accepts standalone skills (`code-review`) or namespaced plugin skills (`plugin:skill`).

Example: `/dev-flow add code-review`

### `/dev-flow remove <skill-name>`

Remove a skill from the auto-load list.

Example: `/dev-flow remove simplify`

### `/dev-flow list`

Display all currently configured auto-load skills with their source.

### `/dev-flow product` — Product Team Management

Manage the multi-persona product discussion team.

Sub-commands:

| Command | Description |
|---------|-------------|
| `/dev-flow product list` | List current product team members with their perspectives |
| `/dev-flow product add <name>` | Add a new product persona. You will be prompted for their perspective/label |
| `/dev-flow product remove <name>` | Remove a product persona |
| `/dev-flow product preset default` | Reset to default 5-role team (Pragmatic, User Advocate, Innovator, Business, Tech-Aware) |
| `/dev-flow product preset lean` | Use lean 2-role team (Pragmatic + User Advocate) |

### `/dev-flow strict on|off`

Toggle strict mode. When ON, any pipeline phase that fails blocks progression to the next phase. When OFF, phases that fail issue warnings but allow continuation.

### `/dev-flow reset`

Reset all configuration to factory defaults. Prompts for confirmation before executing.

## Configuration File

All settings are persisted to `~/.claude/dev-flow.local.md` (user-global). This file uses YAML frontmatter for structured data and Markdown body for freeform notes. It is safe to edit manually if preferred.

## Examples

```bash
# Quick setup: add code review to pipeline
/dev-flow add code-review

# Build a product team
/dev-flow product preset default
/dev-flow product add security-expert

# Lock it down
/dev-flow strict on

# See what's active
/dev-flow
```
