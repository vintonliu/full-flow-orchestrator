# full-flow-orchestrator

全流程开发编排技能 —— 将 6 个阶段（需求 → 设计 → 开发 → 测试 → 交付 → 复盘）映射到现成的 product/engineering/PM 技能，编排它们完成端到端交付。

## 概述

这是一个**编排型技能**。它本身不执行具体开发任务，而是在正确的时间调用正确的子技能，协调整个交付管线。

### 触发场景

- "帮我开发 X"、"从头开始做一个项目"
- "规划这个功能的完整开发流程"
- "帮我跑一遍从需求到上线的全流程"
- "搞一个完整项目"
- 任何涉及多个阶段、需要完整交付管线的请求

> 简单的单步任务（改个 bug、加个小功能）不需要触发此技能。

## 六阶段管线

| 阶段 | 目的 | 典型技能 |
|------|------|---------|
| **Stage 1: 需求** | 明确做什么、为什么做、为谁做 | `product-discovery`, `spec-to-repo` |
| **Stage 2: 设计** | 决定怎么做——架构、UI/UX、技术方案 | `senior-architect`, `writing-plans` |
| **Stage 3a: 实施计划** | 输出实施计划，用户审核 | _(自主编写)_ |
| **Stage 3b: 审核门** | 用户确认后进入编码 | _(用户确认)_ |
| **Stage 3c: 编码** | 按计划执行开发 | `executing-plans`, `senior-fullstack` |
| **Stage 4: 测试** | 代码审查、QA、安全测试 | `code-reviewer`, `senior-qa` |
| **Stage 5: 交付** | 合入主分支并发布 | `commit-push-pr`, `release-manager` |
| **Stage 6: 复盘** | 总结经验和教训 | `meeting-analyzer`, `decision-logger` |

## 核心原则

1. **按需裁剪** — 每个阶段都是可选的，小修补跳过需求和复盘
2. **文件化产出** — 每个阶段的文档保存为版本化文件（v1 → v2 → v3）
3. **质量门** — 进入新阶段前检查上一阶段的可接受性
4. **Karpathy 原则** — 思考先行、简洁优先、外科式改动、目标驱动

## 产出物文档命名

所有非代码产出物按以下命名规则存放：

| 阶段 | 产出物路径 |
|------|-----------|
| 需求 | `docs/requirements/requirements-v1.md` |
| 设计 | `docs/design/design-v1.md` |
| 实施计划 | `docs/implementation-plan/impl-plan-v1.md` |
| 开发记录 | `docs/development-notes/dev-notes-v1.md` |
| 测试报告 | `docs/test-report/test-report-v1.md` |
| 复盘报告 | `docs/retrospective/retro-v1.md` |

## 依赖技能安装

本技能是编排型技能，需要以下插件包中的子技能来完成各阶段工作。这些插件来自不同的 **marketplace（技能市场）**，需先添加 marketplace 源，再安装具体插件。

### 配置 marketplace 源

根据你使用的 Claude Code 环境，可能已预置部分 marketplace。如需手动添加：

```bash
# 官方市场（通常已内置）
claude plugins marketplace add claude-plugins-official

# Claude Code Skills 社区市场
claude plugins marketplace add claude-code-skills
```

### 安装依赖插件

```bash
# ── 编排执行（官方 claude-plugins-official）──
claude plugins install superpowers@claude-plugins-official
# 包含：brainstorming, writing-plans, executing-plans,
#       test-driven-development, dispatching-parallel-agents,
#       subagent-driven-development, verification-before-completion,
#       requesting-code-review, finishing-a-development-branch

# ── 工程开发（社区 claude-code-skills）──
claude plugins install engineering-skills@claude-code-skills
# 包含：senior-architect, code-reviewer, senior-qa, security-pen-testing,
#       adversarial-reviewer, senior-fullstack, senior-backend, senior-frontend
claude plugins install engineering-advanced-skills@claude-code-skills
# 包含：release-manager

# ── 产品/设计（社区 claude-code-skills）──
claude plugins install product-skills@claude-code-skills
# 包含：product-discovery, product-strategist, competitive-teardown,
#       spec-to-repo, ux-researcher-designer, ui-design-system
claude plugins install frontend-design@claude-plugins-official
# 包含：frontend-design（UI 设计）
claude plugins install figma@claude-plugins-official
# 包含：figma-implement-design, figma-generate-design（需要 Figma OAuth 认证）

# ── 项目管理（社区 claude-code-skills）──
claude plugins install pm-skills@claude-code-skills
# 包含：senior-pm, jira-expert, meeting-analyzer, scrum-master, 等
claude plugins install commit-commands@claude-plugins-official
# 包含：commit-push-pr, commit

# ── 高管/决策（社区 claude-code-skills）──
claude plugins install c-level-skills@claude-code-skills
# 包含：decision-logger
```

### 验证安装

```bash
claude plugins list
```

确保列表中所有插件状态为 `✔ enabled`。缺少某个插件时，对应阶段的技能将不可用，编排器会自动跳过或降级处理。

### Marketplace 仓库参考

| Marketplace | GitHub 仓库 | 说明 |
|-------------|-------------|------|
| `claude-plugins-official` | [anthropics/claude-plugins-official](https://github.com/anthropics/claude-plugins-official) | Anthropic 官方技能市场 |
| `claude-code-skills` | [alirezarezvani/claude-skills](https://github.com/alirezarezvani/claude-skills) | 社区技能市场 |

### 从 GitHub 直接安装

如果 marketplace 未配置，你也可以从 GitHub 直接安装插件：

```bash
claude plugins install github:anthropics/claude-plugins-official
claude plugins install github:alirezarezvani/claude-skills
```

> 注意：部分插件需要额外的 MCP 服务器认证（如 `figma` 需要 Figma OAuth）。运行时回提示你完成认证流程。

## 使用方法

在 Claude Code 中直接输入触发场景中的任意语句，技能将自动加载并启动 6 阶段管线。你也可以显式调用：

```
/full-flow-orchestrator <你的需求描述>
```
