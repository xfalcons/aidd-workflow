# AIDD-Workflow Design Document

> **AI-Driven Development Workflow** — a skills-based methodology that merges AIDLC's comprehensive lifecycle with Superpowers' modular, token-efficient delivery.

## Design Principles

1. **Skills as the unit of delivery** — Every capability is a self-contained SKILL.md, loaded on-demand
2. **Skill-based routing** — No central orchestrator; each skill knows its prerequisites and next skill
3. **Lightweight state** — `aidd-state.md` tracks phase/stage progression (~50 lines)
4. **Adaptive depth** — Simple fixes skip unnecessary stages; complex systems get full treatment
5. **TDD-first construction** — Every code task follows RED→GREEN→REFACTOR
6. **Evidence before claims** — No completion without fresh verification
7. **Claude Code native** — Uses Skill tool, hooks, subagents, worktrees

## Instruction Priority

```
1. User's explicit instructions (CLAUDE.md, direct requests) — highest
2. AIDD skills — override default system behavior
3. Default system prompt — lowest
```

---

## Skill Inventory (22 Skills)

### INCEPTION Phase Skills

| # | Skill | Source | Triggers When |
|---|-------|--------|---------------|
| 1 | `using-aidd` | Superpowers `using-superpowers` + AIDLC routing | Start of any conversation — establishes routing |
| 2 | `workspace-detection` | AIDLC `workspace-detection.md` | New project or session resume |
| 3 | `reverse-engineering` | AIDLC `reverse-engineering.md` | Brownfield codebase detected |
| 4 | `brainstorming` | Superpowers `brainstorming` + AIDLC depth | Feature design, approach exploration |
| 5 | `requirements-analysis` | AIDLC `requirements-analysis.md` | Requirements need gathering/validation |
| 6 | `user-stories` | AIDLC `user-stories.md` | User-facing features, complex business logic |
| 7 | `application-design` | AIDLC `application-design.md` | New components, services, or dependencies |
| 8 | `writing-plans` | Superpowers `writing-plans` + AIDLC | After design approval, before implementation |
| 9 | `units-generation` | AIDLC `units-generation.md` | Complex system needing decomposition |

### CONSTRUCTION Phase Skills

| # | Skill | Source | Triggers When |
|---|-------|--------|---------------|
| 10 | `test-driven-development` | Superpowers `test-driven-development` | Any code task |
| 11 | `functional-design` | AIDLC `functional-design.md` | Business logic needs detailed design |
| 12 | `nfr-design` | AIDLC `nfr-design.md` + `nfr-requirements.md` | Performance, security, scalability concerns |
| 13 | `infrastructure-design` | AIDLC `infrastructure-design.md` | Cloud/infra mapping needed |
| 14 | `code-generation` | AIDLC `code-generation.md` + Superpowers TDD | After design, generating actual code |
| 15 | `subagent-driven-development` | Superpowers `subagent-driven-development` | Independent tasks, same session |
| 16 | `executing-plans` | Superpowers `executing-plans` | Sequential plan execution |
| 17 | `systematic-debugging` | Superpowers `systematic-debugging` | Bugs, errors, unexpected behavior |

### CROSS-CUTTING Skills

| # | Skill | Source | Triggers When |
|---|-------|--------|---------------|
| 18 | `verification-before-completion` | Superpowers | Before claiming any work done |
| 19 | `using-git-worktrees` | Superpowers | Needing workspace isolation |
| 20 | `finishing-a-development-branch` | Superpowers | Branch ready to integrate |
| 21 | `code-review` | Superpowers `requesting-code-review` + `receiving-code-review` | Code needs review |
| 22 | `writing-skills` | Superpowers `writing-skills` | Creating/editing skills |

---

## Adaptive Flow Matrix

The workflow adapts based on request complexity. Each row shows which skills execute.

| Request Type | Always | Conditional | Skipped |
|-------------|--------|-------------|---------|
| **Simple bug fix** | `workspace-detection` → `brainstorming`(minimal) → `writing-plans` → `executing-plans` → `verification` | `systematic-debugging` (if needed) | requirements, stories, design, NFR, infra, units |
| **Feature addition** | `workspace-detection` → `brainstorming` → `requirements-analysis` → `writing-plans` → `subagent-driven-development` → `verification` | `user-stories`, `application-design` | reverse-engineering (greenfield), units, NFR |
| **New service/system** | Full inception → `units-generation` → per-unit loop (`functional-design` → `code-generation` with TDD) → `verification` | `nfr-design`, `infrastructure-design` | — |
| **System migration** | Full lifecycle with `reverse-engineering` → all construction skills per unit | All | — |

---

## Directory Structure

```
aidd-workflow/
├── skills/                                    # 22 skills
│   ├── using-aidd/
│   │   └── SKILL.md
│   ├── workspace-detection/
│   │   └── SKILL.md
│   ├── reverse-engineering/
│   │   └── SKILL.md
│   ├── brainstorming/
│   │   └── SKILL.md
│   ├── requirements-analysis/
│   │   └── SKILL.md
│   ├── user-stories/
│   │   └── SKILL.md
│   ├── application-design/
│   │   └── SKILL.md
│   ├── writing-plans/
│   │   └── SKILL.md
│   ├── units-generation/
│   │   └── SKILL.md
│   ├── test-driven-development/
│   │   ├── SKILL.md
│   │   └── testing-anti-patterns.md
│   ├── functional-design/
│   │   └── SKILL.md
│   ├── nfr-design/
│   │   └── SKILL.md
│   ├── infrastructure-design/
│   │   └── SKILL.md
│   ├── code-generation/
│   │   └── SKILL.md
│   ├── subagent-driven-development/
│   │   ├── SKILL.md
│   │   ├── implementer-prompt.md
│   │   ├── spec-reviewer-prompt.md
│   │   └── code-quality-reviewer-prompt.md
│   ├── executing-plans/
│   │   └── SKILL.md
│   ├── systematic-debugging/
│   │   ├── SKILL.md
│   │   ├── root-cause-tracing.md
│   │   ├── defense-in-depth.md
│   │   └── condition-based-waiting.md
│   ├── verification-before-completion/
│   │   └── SKILL.md
│   ├── using-git-worktrees/
│   │   └── SKILL.md
│   ├── finishing-a-development-branch/
│   │   └── SKILL.md
│   ├── code-review/
│   │   ├── SKILL.md
│   │   └── code-reviewer.md
│   └── writing-skills/
│       ├── SKILL.md
│       └── anthropic-best-practices.md
│
├── hooks/                                     # Claude Code integration
│   ├── hooks.json
│   └── session-start
│
├── extensions/                                # Opt-in cross-cutting rules
│   ├── security/
│   │   ├── security-baseline.md
│   │   └── security-baseline.opt-in.md
│   ├── resiliency/
│   │   ├── resiliency-baseline.md
│   │   └── resiliency-baseline.opt-in.md
│   └── property-based-testing/
│       ├── property-based-testing.md
│       └── property-based-testing.opt-in.md
│
├── references/                                # Supplementary docs
│   └── skill-authoring-guide.md
│
└── tests/                                     # Skill test infrastructure
    └── claude-code/
        ├── test-helpers.sh
        └── run-skill-tests.sh
```

---

## State Tracking: `aidd-state.md`

Lightweight state file created in the project root under `aidd-docs/`:

```markdown
# AIDD Workflow State

**Project**: [name]
**Type**: [greenfield|brownfield]
**Started**: [ISO date]
**Current Phase**: [inception|construction|operations]
**Current Stage**: [stage-name]

## Completed Stages
- [x] workspace-detection — [date]
- [x] brainstorming (minimal) — [date]

## Skipped Stages
- reverse-engineering — greenfield project
- user-stories — internal refactoring, no user impact

## Extension Configuration
- security-baseline: [enabled|disabled|not-applicable]
- resiliency-baseline: [enabled|disabled|not-applicable]
- property-based-testing: [enabled|disabled|not-applicable]

## Generated Artifacts
- [list of key artifacts with paths]
```

---

## Hook Configuration

### `hooks/hooks.json`

```json
{
  "hooks": {
    "SessionStart": [
      {
        "type": "command",
        "command": "bash hooks/session-start"
      }
    ]
  }
}
```

### `hooks/session-start`

Outputs JSON with `additional_context` containing a brief reminder to invoke `using-aidd` skill when appropriate.

---

## Extension System

Extensions are NOT skills. They are cross-cutting rule files:

- **Opt-in files** (`*.opt-in.md`): Lightweight prompts presented during `requirements-analysis`
- **Rule files** (`*.md`): Full blocking constraints loaded only when user opts in
- **Enforcement**: Checked during relevant skill execution, blocking findings prevent stage completion

---

## Per-Skill Design Summary

### 1. `using-aidd` (Entry Point)
- **Size target**: ~100 lines
- **From**: Superpowers `using-superpowers` (priority, red flags) + AIDLC routing
- **Contains**: Instruction priority, skill discovery rules, phase→skill routing table, red flags, `<SUBAGENT-STOP>` directive
- **Does NOT contain**: Phase details, stage execution logic

### 2. `workspace-detection`
- **Size target**: ~150 lines
- **From**: AIDLC `workspace-detection.md` + `session-continuity.md`
- **Contains**: Greenfield/brownfield detection, state file creation/resume, artifact discovery
- **Next skill**: `reverse-engineering` (brownfield) or `brainstorming` (greenfield)

### 3. `reverse-engineering`
- **Size target**: ~200 lines
- **From**: AIDLC `reverse-engineering.md`
- **Contains**: Multi-package discovery, business overview, architecture docs, code structure, API docs
- **Next skill**: `brainstorming`

### 4. `brainstorming`
- **Size target**: ~200 lines
- **From**: Superpowers `brainstorming` + AIDLC adaptive depth
- **Contains**: Context exploration, 2-3 approach proposals, approval gate, spec self-review, adaptive depth indicators
- **Next skill**: `requirements-analysis` or directly to `writing-plans` (simple cases)

### 5. `requirements-analysis`
- **Size target**: ~200 lines
- **From**: AIDLC `requirements-analysis.md` + `overconfidence-prevention.md`
- **Contains**: Intent analysis, depth determination (minimal/standard/comprehensive), question categories, extension opt-in presentation
- **Next skill**: `user-stories` (if applicable) or `application-design`

### 6. `user-stories`
- **Size target**: ~200 lines
- **From**: AIDLC `user-stories.md`
- **Contains**: Intelligent assessment (when stories add value), Part 1 Planning, Part 2 Generation, acceptance criteria
- **Next skill**: `application-design`

### 7. `application-design`
- **Size target**: ~200 lines
- **From**: AIDLC `application-design.md`
- **Contains**: Component identification, method signatures, service definitions, dependency mapping
- **Next skill**: `writing-plans` or `units-generation` (complex systems)

### 8. `writing-plans`
- **Size target**: ~200 lines
- **From**: Superpowers `writing-plans` + AIDLC `workflow-planning.md`
- **Contains**: Bite-sized tasks (2-5 min each), no placeholders, TDD-integrated steps, file structure mapping, plan self-review
- **Next skill**: `subagent-driven-development` or `executing-plans`

### 9. `units-generation`
- **Size target**: ~200 lines
- **From**: AIDLC `units-generation.md`
- **Contains**: Work decomposition, dependency matrix, story-to-unit mapping, code organization strategy
- **Next skill**: Per-unit construction loop

### 10. `test-driven-development`
- **Size target**: ~200 lines + anti-patterns reference
- **From**: Superpowers `test-driven-development` (port directly)
- **Contains**: Iron law, RED→GREEN→REFACTOR cycle, anti-patterns, red flags
- **Used by**: `code-generation`, `subagent-driven-development`, `executing-plans`

### 11. `functional-design`
- **Size target**: ~200 lines
- **From**: AIDLC `functional-design.md`
- **Contains**: Business logic model, domain entities, business rules, validation logic
- **Next skill**: `nfr-design` or `code-generation`

### 12. `nfr-design`
- **Size target**: ~200 lines
- **From**: AIDLC `nfr-design.md` + `nfr-requirements.md` (merged)
- **Contains**: Performance/scalability/security patterns, tech stack decisions, logical components
- **Next skill**: `infrastructure-design` or `code-generation`

### 13. `infrastructure-design`
- **Size target**: ~200 lines
- **From**: AIDLC `infrastructure-design.md`
- **Contains**: Cloud service mapping, deployment architecture, shared infrastructure
- **Next skill**: `code-generation`

### 14. `code-generation`
- **Size target**: ~200 lines
- **From**: AIDLC `code-generation.md` + Superpowers TDD integration
- **Contains**: Two-part (plan + generate), TDD-enforced steps, brownfield modify patterns
- **Next skill**: `subagent-driven-development` or `verification-before-completion`

### 15. `subagent-driven-development`
- **Size target**: ~250 lines + prompt files
- **From**: Superpowers `subagent-driven-development` (port directly)
- **Contains**: Fresh subagent per task, two-stage review, context isolation, continuous execution

### 16. `executing-plans`
- **Size target**: ~150 lines
- **From**: Superpowers `executing-plans` (port directly)
- **Contains**: Sequential plan execution, TDD enforcement, self-review

### 17. `systematic-debugging`
- **Size target**: ~250 lines + technique files
- **From**: Superpowers `systematic-debugging` (port directly)
- **Contains**: Root cause tracing, defense-in-depth, condition-based waiting, hypothesis testing

### 18. `verification-before-completion`
- **Size target**: ~100 lines
- **From**: Superpowers `verification-before-completion` (port directly)
- **Contains**: Identify→Run→Read→Verify→THEN claim gate function

### 19. `using-git-worktrees`
- **Size target**: ~150 lines
- **From**: Superpowers `using-git-worktrees` (port directly)
- **Contains**: Isolation detection, native tool preference, provenance-based cleanup

### 20. `finishing-a-development-branch`
- **Size target**: ~150 lines
- **From**: Superpowers `finishing-a-development-branch` (port directly)
- **Contains**: Test verification, base branch detection, structured options

### 21. `code-review`
- **Size target**: ~150 lines
- **From**: Superpowers `requesting-code-review` + `receiving-code-review` (merged)
- **Contains**: Review request format, review reception, feedback integration

### 22. `writing-skills`
- **Size target**: ~200 lines + best practices reference
- **From**: Superpowers `writing-skills` (port directly)
- **Contains**: TDD for documentation, skill types, directory structure, CSO

---

## Implementation Priority

### Phase 1: Core (8 skills)
These are the minimum viable workflow:
1. `using-aidd` — Entry point
2. `workspace-detection` — State management
3. `brainstorming` — Design
4. `writing-plans` — Planning
5. `test-driven-development` — Quality gate
6. `code-generation` — Implementation
7. `subagent-driven-development` — Execution
8. `verification-before-completion` — Completion gate

### Phase 2: Inception Depth (4 skills)
9. `requirements-analysis` — Requirements rigor
10. `user-stories` — Story-driven planning
11. `application-design` — Component design
12. `units-generation` — Work decomposition

### Phase 3: Construction Depth (3 skills)
13. `reverse-engineering` — Brownfield analysis
14. `functional-design` — Business logic
15. `nfr-design` — Non-functional requirements
16. `infrastructure-design` — Cloud/infra

### Phase 4: Cross-cutting (5 skills)
17. `executing-plans` — Sequential execution
18. `systematic-debugging` — Debugging
19. `using-git-worktrees` — Isolation
20. `finishing-a-development-branch` — Integration
21. `code-review` — Review
22. `writing-skills` — Meta-skill

### Phase 5: Extensions & Tests
- Security baseline extension
- Resiliency baseline extension
- Property-based testing extension
- Integration test suite
- Skill triggering tests
