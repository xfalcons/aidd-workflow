---
name: code-generation
description: Use when ready to generate code from an approved plan — enforces TDD per task, tracks progress with checkboxes, and requires verification before completion
---

# Code Generation

Generate code by executing an approved plan's tasks, following TDD for each one.

**Core principle:** Execute the plan exactly. Every task follows RED→GREEN→REFACTOR. No task is complete until verified.

## Prerequisites

- An approved implementation plan (from `aidd:writing-plans`)
- Design artifacts if applicable (from `aidd:functional-design`, `aidd:nfr-design`)
- Workspace state file (`aidd-docs/aidd-state.md`)

## Overview

Code generation has two integrated parts:
- **Part 1 — Planning**: Create a detailed code generation plan (if not already in implementation plan)
- **Part 2 — Generation**: Execute tasks following TDD

For brownfield projects, "generate" means modify existing files when appropriate, not create duplicates.

## Code Location Rules

- **Application code**: Workspace root only (NEVER in `aidd-docs/`)
- **Documentation**: `aidd-docs/` only (markdown summaries)
- Read workspace root from `aidd-docs/aidd-state.md` before generating code

**Structure patterns by project type:**
- **Brownfield**: Use existing structure (e.g., `src/main/java/`, `lib/`, `pkg/`)
- **Greenfield single unit**: `src/`, `tests/`, `config/` in workspace root
- **Greenfield multi-unit (microservices)**: `{unit-name}/src/`, `{unit-name}/tests/`
- **Greenfield multi-unit (monolith)**: `src/{unit-name}/`, `tests/{unit-name}/`

## Brownfield File Modification Rules

- Check if file exists before generating
- If exists: Modify in-place (never create copies like `ClassName_modified.java`)
- If doesn't exist: Create new file
- Verify no duplicate files after generation

## Per-Task TDD Cycle

For each task in the plan:

```
RED:
1. Write the failing test for this task
2. Run test — confirm it fails for the right reason (feature missing, not typo)

GREEN:
3. Write minimal implementation to pass the test
4. Run test — confirm it passes
5. Run ALL tests — confirm nothing else broke

REFACTOR:
6. Clean up (remove duplication, improve names, extract helpers)
7. Run ALL tests — confirm still green

COMPLETE:
8. Commit with descriptive message
9. Mark task [x] in the plan
10. Update aidd-docs/aidd-state.md
```

**Required skill:** Use `aidd:test-driven-development` for the full TDD methodology.
**Required skill:** Use `aidd:verification-before-completion` before claiming any task done.

## Execution Modes

### Mode 1: Subagent-Driven (recommended for multi-task plans)

Use `aidd:subagent-driven-development`:
- Fresh subagent per task with isolated context
- Two-stage review after each task (spec compliance + code quality)
- Continuous execution without pausing between tasks

### Mode 2: Inline (for simple or single-task plans)

Execute tasks directly in this session:
1. Read the plan, identify next uncompleted task
2. Follow the per-task TDD cycle above
3. Self-review after each task
4. Update checkboxes immediately
5. Continue to next task

### Mode 3: Executing Plans (for sequential batch execution)

Use `aidd:executing-plans`:
- Batch execution with checkpoints for review

## Progress Tracking

### Plan-Level Checkboxes
- Mark each step `[x]` IMMEDIATELY after completing it
- This must happen in the SAME interaction where the work is completed

### State File Updates
After completing each task, update `aidd-docs/aidd-state.md`:
```markdown
## Completed Stages
- [x] code-generation — task N: [task name] — [date]
```

## Automation-Friendly Code Rules

When generating UI code, ensure elements are automation-friendly:
- Add `data-testid` attributes to interactive elements
- Use consistent naming: `{component}-{element-role}`
- Keep `data-testid` values stable across code changes

## Completion

After all tasks in the plan are complete:

1. Run full test suite — ALL must pass
2. Run linter — 0 errors
3. Verify against spec requirements (line-by-line checklist)
4. Use `aidd:verification-before-completion` before claiming done
5. Present completion summary with file list

**Completion message format:**
```
💻 Code Generation Complete

• Modified: [list modified files with paths]
• Created: [list created files with paths]
• Tests: [N/N passing]

📋 Review the generated code, then:
🔧 Request Changes — ask for modifications
✅ Continue — approve and proceed to next stage
```

## Red Flags — STOP

- Writing code before writing the failing test
- Marking a task complete without running tests
- Creating duplicate files (`ClassName_modified.*`)
- Putting application code in `aidd-docs/`
- Deviating from the plan without user approval
- Skipping the self-review
- Claiming completion without fresh verification evidence
