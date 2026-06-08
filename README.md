# AIDD-Workflow

> **AI-Driven Development Workflow** — a skills-based Claude Code plugin that merges [AWS AI-DLC's](https://github.com/awslabs/aidlc-workflows) comprehensive lifecycle with [Superpowers'](https://github.com/obra/superpowers) modular, token-efficient delivery.

AIDD gives you the best of both worlds: AIDLC's structured inception→construction→operations phases with adaptive depth, combined with Superpowers' on-demand skills, TDD enforcement, subagent-driven execution, and evidence-first verification.

## How It Works

AIDD is a **Claude Code plugin** with **22 skills** organized into three lifecycle phases. Skills are loaded on-demand — you don't pay token costs for capabilities you aren't using. Each skill knows its prerequisites and what comes next, so the workflow routes itself based on your project's complexity.

### Adaptive Depth

Not every project needs every stage. AIDD adapts automatically:

| Request | Skills Used | Skipped |
|---------|-------------|---------|
| **Simple bug fix** | brainstorming (minimal) → writing-plans → executing-plans → verification | 15 skills |
| **New feature** | brainstorming → requirements-analysis → user-stories → writing-plans → subagent-driven → verification | 10 skills |
| **New service/system** | Full inception → units-generation → per-unit construction → verification | Nothing |

### The Full Flow

```
INCEPTION
  workspace-detection → reverse-engineering (brownfield only)
  → brainstorming → requirements-analysis → user-stories
  → application-design → units-generation → writing-plans

CONSTRUCTION (per unit)
  functional-design → nfr-design → infrastructure-design
  → code-generation (TDD enforced)
  → subagent-driven-development (two-stage review)
  → verification-before-completion
```

## Installation

### Option 1: Skills-Directory Plugin (Recommended)

This is the simplest install — no marketplace needed.

```bash
# Clone or copy into your personal skills directory
git clone <repo-url> ~/.claude/skills/aidd-workflow
```

That's it. On the next Claude Code session, the plugin loads automatically as `aidd-workflow@skills-dir`. It appears in `/plugin list` with no additional steps.

**For a team** (shared via project repository):

```bash
# Copy into the project's .claude/skills/ directory
cp -r aidd-workflow/ /path/to/project/.claude/skills/aidd-workflow/
```

Project-scope plugins load after the workspace trust dialog is accepted.

### Option 2: Install via Plugin Directory

```bash
# Point Claude Code at the plugin directory
claude --plugin-dir /path/to/aidd-workflow
```

This loads the plugin for the duration of that session.

### Option 3: Manual Plugin Install

```bash
# Copy to any location and add to settings
cp -r aidd-workflow/ /path/to/plugins/aidd-workflow/
```

Then add to `~/.claude/settings.json` (user scope) or `.claude/settings.json` (project scope):

```json
{
  "enabledPlugins": {
    "aidd-workflow": true
  }
}
```

### Verifying Installation

Start Claude Code and check:

```
> /plugin list
```

You should see `aidd-workflow` with 22 skills registered. You can also run:

```
> claude plugin details aidd-workflow
```

To see the full component inventory and projected token costs.

## Usage

### Starting a New Project

```
> Using AIDD, I want to build a task management API with user authentication
```

AIDD will automatically:
1. Detect workspace state (greenfield)
2. Brainstorm the design with you
3. Gather requirements and user stories
4. Design the application architecture
5. Break work into units
6. Create an implementation plan
7. Execute with TDD and subagent reviews
8. Verify everything works

### Working on an Existing Codebase

```
> Using AIDD, add a password reset feature to the auth service
```

AIDD will:
1. Detect existing code (brownfield)
2. Reverse-engineer the codebase (if not already done)
3. Brainstorm the approach
4. Analyze requirements for the new feature
5. Plan and execute with TDD

### Quick Bug Fix

```
> Using AIDD, fix the login timeout issue
```

AIDD will use minimal depth:
1. Brainstorm the fix (brief)
2. Write a plan
3. Execute with TDD
4. Verify the fix

## Skills Reference

### Inception (9 skills)

| Skill | Purpose |
|-------|---------|
| `using-aidd` | Entry point — skill discovery, routing, priority |
| `workspace-detection` | Greenfield/brownfield detection, session resume |
| `reverse-engineering` | Existing codebase analysis (brownfield only) |
| `brainstorming` | Design exploration with 2-3 approaches |
| `requirements-analysis` | Adaptive requirements gathering |
| `user-stories` | INVEST-compliant stories with acceptance criteria |
| `application-design` | Component, service, and dependency design |
| `units-generation` | Work decomposition with dependency matrix |
| `writing-plans` | Bite-sized TDD-integrated implementation plans |

### Construction (7 skills)

| Skill | Purpose |
|-------|---------|
| `test-driven-development` | Iron law: no code without failing test first |
| `functional-design` | Business logic, domain models, business rules |
| `nfr-design` | Non-functional requirements + design patterns |
| `infrastructure-design` | Cloud/infrastructure mapping |
| `code-generation` | TDD-enforced code generation from plans |
| `subagent-driven-development` | Fresh subagent per task, two-stage review |
| `executing-plans` | Sequential plan execution with checkpoints |

### Cross-cutting (6 skills)

| Skill | Purpose |
|-------|---------|
| `systematic-debugging` | Root cause tracing, defense-in-depth |
| `verification-before-completion` | Evidence before claims — always |
| `using-git-worktrees` | Workspace isolation for parallel work |
| `finishing-a-development-branch` | Integration, PR, or cleanup |
| `code-review` | Requesting reviews + receiving feedback |
| `writing-skills` | Meta-skill: TDD for documentation |

## Extensions

AIDD includes an opt-in extension system for cross-cutting rules. Extensions are additional rule files inside the plugin's `extensions/` directory:

| Extension | Purpose |
|-----------|---------|
| **Security baseline** | Encryption, input validation, secrets management, auth |
| **Resiliency baseline** | Fault tolerance, retry patterns, health checks, graceful degradation |
| **Property-based testing** | Round-trip, invariant, idempotency properties |

Extensions are presented as opt-in questions during `requirements-analysis`. When enabled, they become blocking constraints checked at each relevant stage.

## Key Principles

- **TDD is the iron law** — No production code without a failing test first
- **Evidence before claims** — Run the command, read the output, THEN claim the result
- **Skills are loaded on-demand** — You only pay tokens for what you use
- **The workflow adapts** — Simple fixes skip 15 stages; complex systems use them all
- **Human in control** — Every design requires your approval before implementation proceeds
- **Subagents get isolated context** — Fresh agent per task, no session pollution

## Plugin Structure

This plugin follows the [Claude Code plugin structure](https://code.claude.com/docs/en/plugins-reference):

```
aidd-workflow/
├── .claude-plugin/
│   └── plugin.json                  # Plugin manifest
├── skills/                          # 22 skills (auto-discovered)
│   ├── using-aidd/SKILL.md          # Entry point skill
│   ├── workspace-detection/SKILL.md
│   ├── reverse-engineering/SKILL.md
│   ├── brainstorming/SKILL.md
│   ├── requirements-analysis/SKILL.md
│   ├── user-stories/SKILL.md
│   ├── application-design/SKILL.md
│   ├── writing-plans/SKILL.md
│   ├── units-generation/SKILL.md
│   ├── test-driven-development/SKILL.md
│   ├── functional-design/SKILL.md
│   ├── nfr-design/SKILL.md
│   ├── infrastructure-design/SKILL.md
│   ├── code-generation/SKILL.md
│   ├── subagent-driven-development/SKILL.md
│   ├── executing-plans/SKILL.md
│   ├── systematic-debugging/SKILL.md
│   ├── verification-before-completion/SKILL.md
│   ├── using-git-worktrees/SKILL.md
│   ├── finishing-a-development-branch/SKILL.md
│   ├── code-review/SKILL.md
│   └── writing-skills/SKILL.md
├── hooks/
│   ├── hooks.json                   # SessionStart hook
│   └── session-start                # Bootstrap script
├── extensions/                      # Opt-in cross-cutting rules
│   ├── security/
│   ├── resiliency/
│   └── property-based-testing/
└── references/                      # Authoring guides
```

### How the Plugin Loads

1. Claude Code discovers the plugin via `~/.claude/skills/aidd-workflow/.claude-plugin/plugin.json`
2. The `hooks/hooks.json` registers a `SessionStart` hook
3. On session start, `hooks/session-start` injects the `using-aidd` skill content into context
4. Claude uses the `Skill` tool to load other skills on-demand as needed
5. Skills route to each other via `aidd:skill-name` references

## Comparison with Parent Projects

| Aspect | AIDLC | Superpowers | AIDD |
|--------|-------|-------------|------|
| **Delivery** | Monolithic 540-line file | Modular skills | ✅ Skills |
| **Lifecycle phases** | Full (Inception→Construction→Ops) | None | ✅ Full phases |
| **TDD** | Late-stage testing | Iron law | ✅ Iron law |
| **Adaptive depth** | Yes | No | ✅ Yes |
| **Subagents** | No | Yes | ✅ Yes |
| **Reverse engineering** | Yes | No | ✅ Yes |
| **Extensions** | Opt-in rules | None | ✅ Opt-in rules |
| **Debugging** | None | Systematic | ✅ Systematic |
| **Code review** | None | Two-stage | ✅ Two-stage |
| **Token cost** | Heavy (load all) | Light (per skill) | ✅ Light |
