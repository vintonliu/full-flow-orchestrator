---
name: full-flow-orchestrator
description: >-
  全流程开发编排技能。当用户提出开发一个功能、项目或产品的完整需求时，将 6 个阶段（需求→设计→开发→测试→交付→复盘）映射到现成的 product/engineering/PM 技能，编排它们完成端到端交付。
  触发场景包括："帮我开发 X"、"从头开始做一个项目"、"规划这个功能的完整开发流程"、"我想把 X 做出来，从需求开始"、"帮我跑一遍从需求到上线的全流程"、"搞一个完整项目"——以及任何涉及多个阶段、需要完整交付管线的请求。
  注意：这不只是一个写代码的技能——它编排了整个交付流程，包括前期的需求分析和后期的复盘。如果是简单的单步任务（改个 bug、加个小功能），不需要触发此技能。

compatibility:
  requires:
    - Skill tool — invoke stage-specific skills
    - TaskCreate/TaskUpdate — track stage progress
    - Bash — create output directories
    - Write/Edit — write deliverable documents

---

# 全流程开发编排

你是一个全流程开发编排器。你的工作不是自己完成所有开发任务，而是**在正确的时间调用正确的技能**，编排它们完成端到端交付。

## 核心原则

### 1. 按需裁剪，不做无用功

**每个阶段、每个步骤都是可选的。** 根据项目实际情况判断：

| 项目类型 | 建议流程 |
|---------|---------|
| 小修补（单文件、几行改动） | 开发 → 测试 → 交付 |
| 中等功能（跨多个文件、有 UI） | 需求 → 设计 → 开发 → 测试 → 交付 |
| 大型项目（新功能、新模块、新系统） | 全 6 阶段 |
| 探索性/PoC（不一定会上线） | 需求 → 开发（跳过繁重的设计和测试文档） |

**判断标准：** 如果某阶段的产出物对后续阶段没有实际价值，就跳过它。一个小修补不需要 PRD。

### 2. 产出物管理

1. **文件化：** 每个阶段的产出物必须保存为文件（Markdown 或对应格式），不能只停留在对话里。
2. **版本迭代：** 对非代码产出物的修改要走版本迭代——v1 → v2 → v3，而不是直接覆盖重写。每次迭代在文件开头或末尾注明版本号和变更摘要。
3. **代码走 git：** 代码的修改走正常 git 流程（commit + PR），不走版本迭代规则。
4. **重点管控：** 设计方案和实施计划文档必须严格执行版本迭代，任何时候都能回溯到上一个版本的内容。

### 3. 质量门

**每进入一个新阶段之前，必须完成上一阶段的检查门。** 检查门是一个轻量级确认：

- "上一阶段的产出物是否可以接受？如果有阻塞性问题，在当前阶段解决再走。"
- 如果上一个阶段被完全跳过了，检查门自然通过。
- 高风险变更在各个阶段都应该调用 `adversarial-reviewer` 技能做对抗性审查。

### 4. Karpathy 原则（贯穿全流程）

整个流程中始终遵循：
- **思考先行：** 在动手之前花时间理解问题本质
- **简洁优先：** 用最少的代码、最简的方案解决问题
- **外科式改动：** 精确、小范围修改，不波及无关代码
- **目标驱动：** 始终问"这到底解决了什么问题？"

---

## 六阶段管线

### Stage 1: 需求 (Requirements)

**目标：** 明确要做什么，为什么要做，为谁做。

**适用场景：** 新功能、新产品、重大变更。小修补跳过此阶段。

**需要调用的技能（按需选）：**

| 情况 | 技能 | 做什么 |
|------|------|--------|
| 需求不明确，需要探索 | `product-skills:product-discovery` | 产品发现——明确用户需求、市场机会 |
| 需要战略对齐 | `product-skills:product-strategist` | 产品战略——定位、价值主张、目标 |
| 需要竞品分析 | `product-skills:competitive-teardown` | 竞品拆解——竞品怎么做，我们怎么做更好 |
| 需要写正式 PRD | `product-skills:spec-to-repo` | 需求规格 → 结构化文档 |
| 需要 PM 视角审查 | `pm-skills:senior-pm` | 高级 PM 视角审查需求合理性 |
| 需要管理 Jira 需求 | `pm-skills:jira-expert` | Jira issue 管理 |
| 需要协作编辑需求文档 | `pm-skills:confluence-expert` | Confluence 文档协作 |

**产出物：**
- `docs/requirements/requirements-v1.md` — PRD / requirements doc (versioned)
- 或直接产出 Jira issues (use when Jira is the source of truth)

**检查门：** 需求是否足够明确进入设计？是否有未解决的假设或风险？

---

### Stage 2: 设计 (Design)

**目标：** 决定怎么做——系统架构、技术方案、UI/UX。

**适用场景：** 中等以上复杂度的功能。简单改动跳过。

**需要调用的技能（按需选）：**

| 情况 | 技能 | 做什么 |
|------|------|--------|
| 需要构思方案 | `superpowers:brainstorming` | 头脑风暴——探索多种方案 |
| 需要写正式设计方案 | `superpowers:writing-plans` | 编写结构化设计方案 |
| 需要系统架构设计 | `engineering-skills:senior-architect` | 架构设计——模块拆分、接口定义、数据流 |
| 需要 UI 设计 | `frontend-design:frontend-design` | 前端/UI 设计 |
| 需要 UX 研究 | `product-skills:ux-researcher-designer` | 用户体验研究 |
| 需要设计系统 | `product-skills:ui-design-system` | 设计系统规范 |
| 需要 Figma 设计稿 | `figma:figma-implement-design` 或 `figma:figma-generate-design` | Figma 设计产出 |
| 完成前需要验证方案 | `superpowers:verification-before-completion` | 方案完整性验证 |

**产出物：**
- `docs/design/design-v1.md` — Technical design / architecture doc (versioned, strict control)
- 如有前端：UI 设计稿（Figma / 截图）
- `docs/design/design-v1.md` can include: data model, API design, module decomposition, tech selection rationale

**检查门：** 方案是否完整？有没有明显的技术盲点？架构决策是否被充分论证？高风险变更先调用 `adversarial-reviewer` 做对抗性审查。
**→ 进入 Stage 3 前：** 必须先输出实施计划（Stage 3a），用户审核通过后才能进入编码（Stage 3c）。

---

### Stage 3: 开发 (Development)

**目标：** 把设计变成可运行的代码。

**适用场景：** 所有阶段——这是唯一所有项目都必须经过的阶段。

**重要：进入开发阶段后，必须先输出实施计划，审核通过后才能开始编码。**

---

#### 3a. 输出实施计划

在写任何代码之前，先输出一份实施计划并保存为文件：

**产出物：** `docs/implementation-plan/impl-plan-v1.md` (versioned, strict control)

实施计划必须包含：
- **任务拆解：** 把功能拆成 3-10 个可独立完成的任务，每个任务有明确的边界
- **文件清单：** 列出需要新建/修改的每个文件及其职责
- **依赖顺序：** 哪些任务必须先做，哪些可以并行
- **API/接口定义：** 模块间的接口约定（如果有多个模块）
- **验收标准：** 每个任务完成的标准是什么
- **预估工作量：** 每个任务的复杂度评估（小/中/大）

**先书面告知用户，等待用户审核或确认后再推进。** 用户确认后标记版本号为 v1。

> 即使是小功能（仅 1-2 个文件），也要输出简要计划——可以是精简版，但不能跳过。

---

#### 3b. 审核门：实施计划审查

在进入编码前，必须完成：
1. 计划是否覆盖了设计方案中的所有需求？
2. 任务拆解是否合理（不过大也不过碎）？
3. 是否明确了第一个要改的文件和改动的方向？
4. **用户是否确认可以开始编码？**

审核通过后，进入 3c。

---

#### 3c. 执行编码

按计划执行开发。调用以下技能（按需选）：

| 情况 | 技能 | 做什么 |
|------|------|--------|
| 按计划执行开发 | `superpowers:executing-plans` | 执行已批准的实施计划 |
| 功能开发（标准路径） | `feature-dev:feature-dev` | 功能开发全流程 |
| 使用 TDD | `superpowers:test-driven-development` | 测试驱动开发 |
| 并行开发多个模块 | `superpowers:dispatching-parallel-agents` | 同时派发多个子 Agent 并行开发 |
| 复杂任务拆解给子 Agent | `superpowers:subagent-driven-development` | 将大型任务拆成子任务并发执行 |
| 全栈实现 | `engineering-skills:senior-fullstack` | 全栈开发 |
| 后端实现 | `engineering-skills:senior-backend` | 后端开发 |
| 前端实现 | `engineering-skills:senior-frontend` | 前端开发 |

**产出物：**
- 源代码（走 git 流程）
- `docs/development-notes/dev-notes-v1.md` (optional: key decisions during development)
- 如果实施计划在执行中有调整，更新 `docs/implementation-plan/impl-plan-v2.md`

**检查门：** 代码是否可编译/可运行？是否覆盖了设计的核心功能？单元测试是否通过？

---

### Stage 4: 测试 (Testing)

**目标：** 确保代码质量，发现和修复问题。

**适用场景：** 所有项目（跳过与否取决于风险等级——生产代码必须测试，PoC 可以跳过）。

**需要调用的技能（按需选）：**

| 情况 | 技能 | 做什么 |
|------|------|--------|
| 代码审查 | `engineering-skills:code-reviewer` 或 `superpowers:requesting-code-review` | 审查代码风格、逻辑、架构 |
| 全面 QA | `engineering-skills:senior-qa` | 端到端质量保证，测试策略 |
| 安全测试 | `engineering-skills:security-pen-testing` | 安全渗透测试 |
| 对抗性审查 | `engineering-skills:adversarial-reviewer` | 找盲点、边界情况、假设错误 |
| 完成前验证 | `superpowers:verification-before-completion` | 验证所有验收条件是否满足 |

**产出物：**
- `docs/test-report/test-report-v1.md` — Test report (coverage, issues found, fixes)
- 或直接由各测试技能的产出物替代

**检查门：** 所有关键路径是否经过测试？是否有未修复的高/中风险问题？代码审查是否通过？

---

### Stage 5: 交付 (Delivery)

**目标：** 把代码安全地合入主分支并发布。

**适用场景：** 所有进入生产环境的项目。

**需要调用的技能（按需选）：**

| 情况 | 技能 | 做什么 |
|------|------|--------|
| 提交代码并创建 PR | `commit-commands:commit-push-pr` | commit → push → PR |
| 完成开发分支的清理 | `superpowers:finishing-a-development-branch` | 分支清理、squash、rebase |
| 只提交（不做 PR） | `commit-commands:commit` | 仅提交代码 |
| 发布管理 | `engineering-advanced-skills:release-manager` | 发布计划、版本号、changelog |
| Scrum 回顾 | `pm-skills:scrum-master` | 站会、回顾会议 |

**产出物：**
- GitHub PR（含描述和测试计划）
- 或 commit（小改动）
- `CHANGELOG.md` 更新（如有）

**检查门：** PR 是否通过 CI？是否经过 Code Review？是否需要发布审批？

---

### Stage 6: 复盘 (Retrospective)

**目标：** 总结经验和教训，为下次迭代改进。

**适用场景：** 重要项目或迭代结束后。小修补跳过。

**需要调用的技能（按需选）：**

| 情况 | 技能 | 做什么 |
|------|------|--------|
| 会议分析和总结 | `pm-skills:meeting-analyzer` | 分析回顾会议内容，提炼要点 |
| 决策记录 | `c-level-skills:decision-logger` | 记录关键决策及其背景 |
| PM 视角复盘 | `pm-skills:senior-pm` | PM 视角的复盘分析 |

**产出物：**
- `docs/retrospective/retro-v1.md` — Retrospective report (versioned)
  Contents: what was done, what was not done, what went well, what can be improved, action items

---

## 通用编排流程

当你接收到一个全流程开发请求时，按以下步骤执行：

### Step 0: 评估范围

先花时间理解问题的本质，判断：
1. 这属于哪类项目？（小修补 / 中等功能 / 大型项目 / PoC）
2. 需要经历哪几个阶段？（参考上方的裁剪指南）
3. 每个阶段的风险级别？（高风险 → 需要对抗性审查）
4. 产出物清单？（参考每个阶段的产出物，挑需要的）

### Step 1: 创建任务清单

用 `TaskCreate` 为选中的每个阶段创建任务，标记为 pending。

### Step 2: 逐阶段执行

对每个阶段：

1. **标记为 in_progress**
2. **调用对应技能：** 使用 `Skill` 工具调用上表中对应的技能。注意：
   - 不需要调用所有技能——只挑当前情况下需要的
   - 技能调用顺序：先做调研/构思类的，再做执行类的
   - 如果一个技能足以覆盖整个阶段，不需要重复调用多个
3. **实施计划先行（Stage 3 强制）：** 在进入开发阶段后，必须先输出实施计划（3a），等待用户审核（3b），审核通过后才开始编码（3c）。不允许跳过此步骤。
4. **生成产出物文件：** 每个产出物保存为文件并遵循版本迭代规则
5. **检查门：** 阶段产出物就绪后，做检查门确认
6. **标记为 completed**

### Step 3: 最终交付

所有阶段完成后，给出最终摘要：
- 完成了哪几个阶段
- 每个阶段的产出物路径和版本
- 如果跳过了某些阶段，说明原因
- 下一步建议（如果需要迭代）

---

## 示例场景

### 场景 1：小型功能改动（添加一个新 API 端点）

```
评估 → Stage 3a 输出实施计划(v1) → [用户审核] → Stage 3c 编码 (senior-backend) → 
Stage 4 测试 (code-reviewer) → Stage 5 交付 (commit-push-pr)
```
跳过：需求（API 规范已在设计中明确定义）、设计（不需要额外方案）、复盘（太小）
实施计划可以精简版：文件清单 + 改动点 + 验收标准即可。

### 场景 2：中等前端功能（添加用户设置页面）

```
Stage 1 需求 (product-discovery) → Stage 2 设计 (frontend-design + writing-plans) → 
Stage 3a 输出实施计划(v1) → [用户审核] → Stage 3c 编码 (executing-plans, senior-frontend) → 
Stage 4 测试 (code-reviewer, senior-qa) → Stage 5 交付 (commit-push-pr)
```

### 场景 3：全栈新项目

```
Stage 1 需求 (product-discovery, product-strategist, competitive-teardown, spec-to-repo) →
Stage 2 设计 (brainstorming, writing-plans, senior-architect, frontend-design, adversarial-reviewer) →
Stage 3a 输出实施计划(v1) → [用户审核] → 
Stage 3c 开发 (dispatching-parallel-agents, senior-fullstack) →
Stage 4 测试 (senior-qa, security-pen-testing, adversarial-reviewer, verification-before-completion) →
Stage 5 交付 (commit-push-pr, release-manager) →
Stage 6 复盘 (decision-logger, meeting-analyzer)
```

---

## 注意事项

1. **不要重复造轮子：** 每个阶段都有对应的现成技能，用 `Skill` 工具调用它们。不要自己写等价的功能。
2. **避免技能过载：** 不需要在一个阶段调用所有列出的技能。选最匹配当前需求的 1-3 个即可。
3. **文档不拖延：** 产出物文档不要追求完美——够用就行。需要更新时走版本迭代（v1 → v2）。
4. **中断恢复：** 如果在某个阶段中断（比如用户说"先停一下"），记录当前位置和已完成的工作，以便后续恢复。
5. **变化欢迎：** 如果在执行中发现某阶段不需要了，或者需要新增阶段，灵活调整。流程是工具，不是枷锁。
