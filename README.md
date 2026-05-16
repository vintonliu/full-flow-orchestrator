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

本技能是编排型技能，需要以下插件包中的子技能来完成各个阶段的工作：

### 必装插件

```bash
# 编排执行能力
claude plugins install superpowers          # brainstorming, writing-plans, executing-plans, 等

# 工程开发能力
claude plugins install engineering-skills   # senior-architect, code-reviewer, senior-qa, 等
claude plugins install engineering-advanced-skills  # release-manager

# 产品/设计能力
claude plugins install product-skills       # product-discovery, spec-to-repo, 等
claude plugins install frontend-design      # frontend-design (UI 设计)
claude plugins install figma                # figma-implement-design, figma-generate-design

# 项目管理能力
claude plugins install pm-skills            # senior-pm, jira-expert, meeting-analyzer, 等
claude plugins install commit-commands      # commit-push-pr, commit

# 高管/决策能力
claude plugins install c-level-skills       # decision-logger
```

### 验证安装

```bash
claude plugins list
```

确保列表中包含以上所有插件。缺少某个插件时，对应阶段的技能将不可用，技能编排时会跳过或降级处理。

> 提示：部分插件可能需要特定的 MCP 服务器（如 `figma` 需要 Figma MCP 认证）。运行时会提示你完成 OAuth 流程。

## 使用方法

在 Claude Code 中直接输入触发场景中的任意语句，技能将自动加载并启动 6 阶段管线。你也可以显式调用：

```
/full-flow-orchestrator <你的需求描述>
```
