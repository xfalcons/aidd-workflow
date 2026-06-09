# AIDD-Workflow

> **AI-Driven Development Workflow** — a skills-based plugin compatible with [Claude Code](https://code.claude.com), [OpenAI Codex](https://developers.openai.com/codex), and [OpenCode](https://open-code.ai) that merges [AWS AI-DLC's](https://github.com/awslabs/aidlc-workflows) comprehensive lifecycle with [Superpowers'](https://github.com/obra/superpowers) modular, token-efficient delivery.

AIDD gives you the best of both worlds: AIDLC's structured inception→construction→operations phases with adaptive depth, combined with Superpowers' on-demand skills, TDD enforcement, subagent-driven execution, and evidence-first verification.

## How It Works

AIDD is a **skills-based plugin** with **22 skills** organized into three lifecycle phases. Skills are loaded on-demand — you don't pay token costs for capabilities you aren't using. Each skill knows its prerequisites and what comes next, so the workflow routes itself based on your project's complexity.

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

### Claude Code

#### Option 1: Skills-Directory Plugin (Recommended)

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

#### Option 2: Install via Plugin Directory

```bash
# Point Claude Code at the plugin directory
claude --plugin-dir /path/to/aidd-workflow
```

This loads the plugin for the duration of that session.

#### Option 3: Manual Plugin Install

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

#### Verifying Installation

Start Claude Code and check:

```
> /plugin list
```

You should see `aidd-workflow` with 22 skills registered.

---

### OpenAI Codex

AIDD is compatible with Codex's plugin system. The repo includes both `.claude-plugin/plugin.json` (for Claude Code) and `.codex-plugin/plugin.json` (for Codex). The skill format, hooks, and SKILL.md frontmatter are identical across both platforms.

#### Option 1: Repo Marketplace (Recommended)

The repo includes a built-in marketplace at `.agents/plugins/marketplace.json`. Clone the repo into your project and point Codex at it:

```bash
# Add as a repo marketplace
codex plugin marketplace add ./aidd-workflow
```

Or manually add to `$REPO_ROOT/.agents/plugins/marketplace.json`:

```json
{
  "name": "aidd-workflow",
  "plugins": [
    {
      "name": "aidd-workflow",
      "source": {
        "source": "local",
        "path": "./aidd-workflow"
      },
      "policy": {
        "installation": "AVAILABLE",
        "authentication": "ON_INSTALL"
      },
      "category": "Developer Tools"
    }
  ]
}
```

#### Option 2: Personal Marketplace

```bash
# Clone into your personal plugins directory
git clone <repo-url> ~/.agents/plugins/aidd-workflow
```

Then add to `~/.agents/plugins/marketplace.json`:

```json
{
  "name": "personal-plugins",
  "plugins": [
    {
      "name": "aidd-workflow",
      "source": {
        "source": "local",
        "path": "./aidd-workflow"
      },
      "policy": {
        "installation": "AVAILABLE",
        "authentication": "ON_INSTALL"
      },
      "category": "Developer Tools"
    }
  ]
}
```

#### Option 3: CLI Install

```bash
codex plugin marketplace add ./path/to/aidd-workflow
```

#### Verifying Installation

```
> /plugins
```

Browse the marketplace, find AIDD Workflow, and select `Install plugin`.

#### Cross-Platform Compatibility

| Aspect | Claude Code | OpenAI Codex | OpenCode |
|--------|-------------|--------------|----------|
| Manifest | `.claude-plugin/plugin.json` | `.codex-plugin/plugin.json` | N/A (skills-only) |
| Skills format | `skills/<name>/SKILL.md` | `skills/<name>/SKILL.md` | `skills/<name>/SKILL.md` |
| Frontmatter | `name` + `description` | `name` + `description` | `name` + `description` |
| Hooks | `hooks/hooks.json` | `hooks/hooks.json` | ❌ Not supported |
| Hook env var | `CLAUDE_PLUGIN_ROOT` | `PLUGIN_ROOT` + `CLAUDE_PLUGIN_ROOT` | N/A |
| Skill loading | `Skill` tool | `skill` tool | `skill` tool |
| SessionStart bootstrap | ✅ Auto-injects | ✅ Auto-injects | ❌ Manual (`AGENTS.md`) |

Both manifests (`.claude-plugin/` and `.codex-plugin/`) are included in this repo. The `hooks/session-start` script auto-detects the platform via environment variables. For OpenCode, skills are portable but the hook is not — use `AGENTS.md` to reference `using-aidd` as the entry point.

---

### OpenCode

OpenCode natively supports the same `SKILL.md` skill format and even searches `~/.claude/skills/` as a built-in skill discovery path. AIDD works with **zero code changes**.

#### Option 1: Claude-Compatible Path (Recommended)

OpenCode automatically discovers skills from `~/.claude/skills/`. If you already installed for Claude Code, it works in OpenCode too — no extra steps.

```bash
git clone <repo-url> ~/.claude/skills/aidd-workflow/skills
```

Wait — OpenCode expects each skill as a direct subdirectory, not nested under a plugin wrapper. The correct install is:

```bash
# OpenCode scans ~/.claude/skills/<name>/SKILL.md
# Clone the repo, then symlink each skill directory
git clone <repo-url> /path/to/aidd-workflow
for skill in /path/to/aidd-workflow/skills/*/; do
  ln -s "$skill" ~/.claude/skills/$(basename "$skill")
done
```

#### Option 2: OpenCode Native Path

```bash
# Clone the repo
git clone <repo-url> /path/to/aidd-workflow

# Symlink skills into OpenCode's native skill directory
mkdir -p ~/.config/opencode/skills
for skill in /path/to/aidd-workflow/skills/*/; do
  ln -s "$skill" ~/.config/opencode/skills/$(basename "$skill")
done
```

#### Option 3: Project-Level

```bash
# Symlink into the project's .opencode/skills/ directory
mkdir -p .opencode/skills
for skill in /path/to/aidd-workflow/skills/*/; do
  ln -s "$skill" .opencode/skills/$(basename "$skill")
done
```

#### OpenCode Skill Discovery Paths

OpenCode searches these locations (in order):

| Path | Scope |
|------|-------|
| `.opencode/skills/<name>/SKILL.md` | Project |
| `~/.config/opencode/skills/<name>/SKILL.md` | Global (OpenCode native) |
| `.claude/skills/<name>/SKILL.md` | Project (Claude-compatible) |
| `~/.claude/skills/<name>/SKILL.md` | Global (Claude-compatible) |
| `.agents/skills/<name>/SKILL.md` | Project (Agent-compatible) |
| `~/.agents/skills/<name>/SKILL.md` | Global (Agent-compatible) |

**Note:** OpenCode discovers skills via the `skill` tool (same as Claude Code's `Skill` tool). The SessionStart hook from `hooks/session-start` does not run on OpenCode — only the skills themselves are portable. The `using-aidd` entry-point skill should be invoked manually or referenced in your `AGENTS.md`.

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

This plugin follows the [Claude Code plugin structure](https://code.claude.com/docs/en/plugins-reference) and is also compatible with the [OpenAI Codex plugin structure](https://developers.openai.com/codex/plugins/build):

```
aidd-workflow/
├── .claude-plugin/
│   └── plugin.json                  # Claude Code manifest
├── .codex-plugin/
│   └── plugin.json                  # OpenAI Codex manifest (with interface metadata)
├── .agents/
│   └── plugins/
│       └── marketplace.json         # Codex marketplace catalog
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
│   └── session-start                # Bootstrap script (cross-platform)
├── extensions/                      # Opt-in cross-cutting rules
│   ├── security/
│   ├── resiliency/
│   └── property-based-testing/
└── references/                      # Authoring guides
```

### How the Plugin Loads

**Claude Code:**
1. Claude Code discovers the plugin via `~/.claude/skills/aidd-workflow/.claude-plugin/plugin.json`
2. The `hooks/hooks.json` registers a `SessionStart` hook
3. On session start, `hooks/session-start` injects the `using-aidd` skill content into context
4. Claude uses the `Skill` tool to load other skills on-demand as needed
5. Skills route to each other via `aidd:skill-name` references

**OpenAI Codex:**
1. Codex discovers the plugin via `.codex-plugin/plugin.json`
2. Skills are registered from `./skills/` — Codex loads `name`/`description` at startup
3. Full SKILL.md content loads on-demand when Codex decides to use a skill
4. The `hooks/hooks.json` registers a `SessionStart` hook (uses `PLUGIN_ROOT` env var)
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
