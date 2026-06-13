---
name: workspace-detection
description: Use when starting a new project or resuming an existing AIDD session - detects workspace state, determines greenfield vs brownfield, and manages session continuity
---

# Workspace Detection

Determine workspace state, detect existing AIDD projects, and manage session continuity.

**Core principle:** Understand before acting. Know what exists before deciding what to build.

## Checklist

1. **Check for existing AIDD session** — look for `aidd-docs/aidd-state.md`
2. **Scan workspace for existing code** — determine greenfield vs brownfield
3. **Create or update state file** — track progress
4. **Route to next skill** — based on findings

## Step 1: Check for Existing Session

Check if `aidd-docs/aidd-state.md` exists:

- **If exists**: This is a resumed session. Read the state file and load context.
- **If not exists**: This is a new session. Continue to workspace scan.

### Session Resume (if state file exists)

<!-- AUDIT: Append to aidd-docs/audit.md:
## Session Resume
- **Timestamp**: [ISO 8601]
- **User Input**: "[user's return message]"
- **AI Response**: "Resuming from [current stage]"
- **Context**: Session resume, loading artifacts from [phase]

---
-->

1. Read `aidd-docs/aidd-state.md` to determine:
   - Current phase and stage
   - Completed stages
   - Skipped stages
   - Extension configuration
2. Load relevant artifacts based on current stage:

| Current Stage | Load These Artifacts |
|---------------|---------------------|
| Reverse engineering | `aidd-docs/inception/reverse-engineering/*` |
| Requirements | `aidd-docs/inception/requirements/*` |
| Design | Requirements + stories + architecture artifacts |
| Construction | ALL prior artifacts + existing code files |

3. Present resume prompt to user:
```
**Welcome back!** Here's your current status:
- **Project**: [name]
- **Phase**: [Inception/Construction]
- **Current Stage**: [stage]
- **Next Step**: [next action]

What would you like to work on?
```

## Step 2: Scan Workspace for Existing Code

Scan the workspace root (NOT inside `aidd-docs/`) for:

- **Source code files**: .java, .py, .js, .ts, .jsx, .tsx, .kt, .go, .rs, .rb, .php, .c, .cpp, .cs, .fs, etc.
- **Build files**: pom.xml, package.json, build.gradle, Cargo.toml, go.mod, etc.
- **Project structure indicators**: src/, lib/, pkg/, app/, etc.

**Record findings:**
```
Workspace State:
- Existing Code: [Yes/No]
- Languages: [list]
- Build System: [if found]
- Project Structure: [Monolith/Microservices/Library/Empty]
```

## Step 3: Determine Project Type and Next Skill

**Greenfield** (no existing code):
- Set flag: `brownfield = false`
- Next skill: `brainstorming`

**Brownfield** (existing code detected):
- Set flag: `brownfield = true`
- Check for existing reverse engineering artifacts in `aidd-docs/inception/reverse-engineering/`
- If artifacts exist and current → next skill: `brainstorming`
- If artifacts missing or stale → next skill: `reverse-engineering` (Phase 2)
- For Phase 1, proceed to `brainstorming` with a note that reverse engineering should run later

## Step 4: Create State File

Create `aidd-docs/aidd-state.md`:

```markdown
# AIDD Workflow State

**Project**: [name or "untitled"]
**Type**: [greenfield|brownfield]
**Started**: [ISO date]
**Current Phase**: [inception|construction]
**Current Stage**: [stage-name]

## Completed Stages
- [x] workspace-detection — [date]

## Skipped Stages
- [list skipped stages with reasons]

## Extension Configuration
- [to be populated during requirements-analysis]

## Generated Artifacts
- aidd-docs/aidd-state.md (this file)
```

Create `aidd-docs/audit.md` if it does not exist:

```markdown
# AIDD Audit Trail

> Append-only log of all interactions. Never overwrite.

---
```

<!-- AUDIT: Append to aidd-docs/audit.md:
## Workspace Detection - Initial Request
- **Timestamp**: [ISO 8601]
- **User Input**: "[complete raw user request - never summarized]"
- **AI Response**: "Created session, scanning workspace"
- **Context**: Session start, new project

---
-->

## Step 5: Present Findings and Proceed

**No user approval required** — workspace detection is informational.

**Brownfield:**
```
🔍 Workspace Detection Complete
• **Type**: Brownfield project
• **Languages**: [detected languages]
• **Build System**: [detected system]
• Proceeding to brainstorming...
```

**Greenfield:**
```
🔍 Workspace Detection Complete
• **Type**: Greenfield project
• Proceeding to brainstorming...
```

Automatically invoke `brainstorming` skill next.

<!-- AUDIT: Append to aidd-docs/audit.md:
## Workspace Detection - Findings
- **Timestamp**: [ISO 8601]
- **User Input**: N/A
- **AI Response**: "[greenfield/brownfield] project detected, languages: [list], build system: [if found]"
- **Context**: Workspace detection complete

---
-->

## Key Principles

- **Non-blocking**: Workspace detection never requires user approval
- **Lightweight state**: Only track what's needed for routing
- **Smart context loading**: On resume, load only artifacts relevant to current stage
- **Audit trail**: Log initial request and findings to `aidd-docs/audit.md` (append-only, created here)
