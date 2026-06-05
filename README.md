# Dev-Flow

A Claude Code plugin that orchestrates a **six-phase role-playing development pipeline**. Every new feature passes through Product → Tech Lead → Developer → QA → Review → Merge, with each phase producing a structured deliverable and pausing for your confirmation.

## Pipeline Overview

```
用户需求 "我要一个登录功能"
    │
    ▼
🎯 Phase 1: 产品团队 (多角色讨论)
    ├─ 👔 务实派  💙 用户派  🚀 创新派  📊 商业派  ⚙️ 技术派
    ├─ 各角色从自己的视角讨论需求
    ├─ 收敛后输出 PRD
    └─ ⏸️ 等待用户确认
    ▼
📋 Phase 2: 研发团队
    ├─ 技术方案设计
    ├─ 任务拆解 + 依赖关系 + 复杂度评估
    └─ ⏸️ 等待用户确认
    ▼
💻 Phase 3: 开发团队
    ├─ 按任务清单编码实现
    ├─ 自检（模式研究、风格匹配、精准修改、一致性）
    └─ ⏸️ 等待用户确认
    ▼
🧪 Phase 4: 测试团队
    ├─ 验收标准验证 + 回归检查 + 边界覆盖
    ├─ 输出测试报告（含 verdict）
    └─ ⏸️ 等待用户确认
    ▼
🔍 Phase 5: 评审委员会
    ├─ 五维度评分（需求对齐、代码质量、测试覆盖、风险、架构）
    ├─ 决策：✅通过 / ⚠️有条件通过 / ❌返工
    └─ ⏸️ 等待用户确认
    ▼
✅ Phase 6: 合入 (或返工到指定阶段)
```

## Key Features

- **全局生效** — 安装在用户级别，所有项目自动应用
- **六阶段角色扮演** — 模拟真实软件开发团队的工作流程
- **多角色产品讨论** — 默认 5 位产品人格从不同视角讨论需求，可自定义
- **每阶段确认** — 每个阶段结束后暂停，你审阅通过才进入下一阶段
- **结构化交付物** — 每阶段产出标准格式文档（PRD、任务清单、测试报告、评审决策）
- **智能分类** — 新功能走完整 6 阶段，Bug 修复走轻量 3 阶段，纯对话跳过
- **交互式配置** — `/dev-flow` 命令管理所有设置

## Installation

```bash
# In Claude Code — install globally (user scope)
/plugin marketplace add Prodigorgeous/dev-flow
/plugin install dev-flow@dev-flow-marketplace --scope user
```

After installation, the plugin activates immediately. No further setup required.

## Configuration

### Interactive (Recommended)

```bash
/dev-flow                          # View current status
/dev-flow add code-review          # Add auto-load skill
/dev-flow product preset default   # Set up product team
/dev-flow strict on                # Enable strict mode
```

### Manual

Create `~/.claude/dev-flow.local.md`:

```yaml
---
auto_load_skills:
  - code-review
  - simplify

product_team:
  - name: pragmatic
    label: 务实派
    perspective: "Ship fast, minimize complexity."
  - name: user-advocate
    label: 用户派
    perspective: "UX above all. Remove friction."
  - name: tech-aware
    label: 技术派
    perspective: "Feasibility, security, compatibility."

strict_mode: false
---
```

## Usage

Just use Claude Code normally. Dev-Flow activates when you request a new feature:

| You say | Pipeline |
|---------|----------|
| "帮我做一个用户登录功能" | Full 6-phase |
| "修复登录按钮不显示的 bug" | Light 3-phase (Dev → QA → Review) |
| "改个拼写错误" | Fast track (self-review) |
| "JWT 是怎么工作的？" | No pipeline |

At each phase, you'll see the structured deliverable and a request for confirmation. Reply "通过" to advance, or specify changes.

## The 5 Default Product Personas

| Role | Label | Focus |
|------|-------|-------|
| 👔 Pragmatic | 务实派 | Complexity, cost, deadlines, simplicity |
| 💙 User Advocate | 用户派 | UX, friction, intuitiveness, user pain |
| 🚀 Innovator | 创新派 | Differentiation, future trends, ambition |
| 📊 Business | 商业派 | Market value, conversion, retention |
| ⚙️ Tech-Aware | 技术派 | Feasibility, compatibility, security |

Customize via `/dev-flow product add/remove` or edit your config file.

## Documentation

| File | Content |
|------|---------|
| `skills/dev-flow/SKILL.md` | Core pipeline instructions |
| `skills/dev-flow/references/default-pipeline.md` | Full pipeline rationale and design philosophy |
| `skills/dev-flow/references/config-guide.md` | Complete configuration reference |
| `skills/dev-flow/examples/dev-flow.local.md` | Template configuration file |

## License

MIT
