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

## Usage

In Claude Code, simply type any trigger phrase and the skill will auto-load the 6-stage pipeline. You can also invoke it explicitly:

```
/full-flow-orchestrator <your requirement description>
```
