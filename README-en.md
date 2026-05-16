# full-flow-orchestrator

A full-flow development orchestration skill — maps a 6-stage pipeline (Requirements → Design → Development → Testing → Delivery → Retrospective) onto existing product/engineering/PM skills, orchestrating them for end-to-end delivery.

## Overview

This is an **orchestration skill**. It does not execute development tasks itself. Instead, it invokes the right sub-skills at the right time, coordinating the entire delivery pipeline.

### Trigger Scenarios

- "Help me build X" / "Start a project from scratch"
- "Plan the full development flow for this feature"
- "Take me through a complete requirements-to-launch pipeline"
- "Build a complete project"
- Any request involving multiple phases that requires an end-to-end delivery pipeline

> Simple one-step tasks (fixing a bug, adding a small feature) do not need this skill.

## The 6-Stage Pipeline

| Stage | Purpose | Typical Skills |
|-------|---------|----------------|
| **Stage 1: Requirements** | Define what, why, and for whom | `product-discovery`, `spec-to-repo` |
| **Stage 2: Design** | Decide how — architecture, UI/UX, tech plan | `senior-architect`, `writing-plans` |
| **Stage 3a: Implementation Plan** | Output implementation plan, await review | _(self-contained)_ |
| **Stage 3b: Review Gate** | User confirms before coding begins | _(user confirmation)_ |
| **Stage 3c: Coding** | Execute the plan | `executing-plans`, `senior-fullstack` |
| **Stage 4: Testing** | Code review, QA, security testing | `code-reviewer`, `senior-qa` |
| **Stage 5: Delivery** | Merge to main and release | `commit-push-pr`, `release-manager` |
| **Stage 6: Retrospective** | Summarize lessons learned | `meeting-analyzer`, `decision-logger` |

## Core Principles

1. **Trim as needed** — every stage is optional; small fixes skip requirements and retrospective
2. **File-based deliverables** — every document is saved as a versioned file (v1 → v2 → v3)
3. **Quality gates** — check previous stage acceptability before entering the next
4. **Karpathy principles** — think first, prefer simplicity, make surgical changes, stay goal-driven

## Deliverable Naming

All non-code deliverables follow this naming convention:

| Stage | Output Path |
|-------|-------------|
| Requirements | `docs/requirements/requirements-v1.md` |
| Design | `docs/design/design-v1.md` |
| Implementation Plan | `docs/implementation-plan/impl-plan-v1.md` |
| Dev Notes | `docs/development-notes/dev-notes-v1.md` |
| Test Report | `docs/test-report/test-report-v1.md` |
| Retrospective | `docs/retrospective/retro-v1.md` |

## Dependency Installation

This is an orchestration skill that requires sub-skills from multiple plugin **marketplaces**. You need to add the relevant marketplace sources and then install the required plugins.

### Add Marketplace Sources

Some marketplaces are pre-configured. To add them manually:

```bash
claude plugins marketplace add claude-plugins-official
claude plugins marketplace add claude-code-skills
```

### Install Required Plugins

```bash
# ── Orchestration & execution (official: claude-plugins-official) ──
claude plugins install superpowers@claude-plugins-official
# Includes: brainstorming, writing-plans, executing-plans,
#           test-driven-development, dispatching-parallel-agents,
#           subagent-driven-development, verification-before-completion,
#           requesting-code-review, finishing-a-development-branch

# ── Engineering (community: claude-code-skills) ──
claude plugins install engineering-skills@claude-code-skills
# Includes: senior-architect, code-reviewer, senior-qa, security-pen-testing,
#           adversarial-reviewer, senior-fullstack, senior-backend, senior-frontend
claude plugins install engineering-advanced-skills@claude-code-skills
# Includes: release-manager

# ── Product & design (community: claude-code-skills) ──
claude plugins install product-skills@claude-code-skills
# Includes: product-discovery, product-strategist, competitive-teardown,
#           spec-to-repo, ux-researcher-designer, ui-design-system
claude plugins install frontend-design@claude-plugins-official
# Includes: frontend-design (UI design)
claude plugins install figma@claude-plugins-official
# Includes: figma-implement-design, figma-generate-design (requires Figma OAuth)

# ── Project management (community: claude-code-skills) ──
claude plugins install pm-skills@claude-code-skills
# Includes: senior-pm, jira-expert, meeting-analyzer, scrum-master, etc.
claude plugins install commit-commands@claude-plugins-official
# Includes: commit-push-pr, commit

# ── Executive / decision (community: claude-code-skills) ──
claude plugins install c-level-skills@claude-code-skills
# Includes: decision-logger
```

### Verify Installation

```bash
claude plugins list
```

Ensure all plugins show `✔ enabled`. Missing plugins will cause the orchestrator to skip or degrade the corresponding stage.

### Marketplace Repository Reference

| Marketplace | GitHub Repository | Description |
|-------------|-------------------|-------------|
| `claude-plugins-official` | [anthropics/claude-plugins-official](https://github.com/anthropics/claude-plugins-official) | Official Anthropic skills |
| `claude-code-skills` | [alirezarezvani/claude-skills](https://github.com/alirezarezvani/claude-skills) | Community skills |

### Install Directly from GitHub

If you haven't configured a marketplace, you can install plugins directly from GitHub:

```bash
claude plugins install github:anthropics/claude-plugins-official
claude plugins install github:alirezarezvani/claude-skills
```

> Note: Some plugins require additional MCP server authentication (e.g., `figma` requires Figma OAuth). The runtime will prompt you to complete the flow when needed.

## Usage

In Claude Code, simply type any trigger phrase and the skill will auto-load the 6-stage pipeline. You can also invoke it explicitly:

```
/full-flow-orchestrator <your requirement description>
```
