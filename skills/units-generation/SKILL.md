---
name: units-generation
description: Use when a complex system needs decomposition into independently developable units of work — creates dependency matrix and story-to-unit mapping
---

# Units Generation

Decompose a system into manageable, independently developable units of work.

**Core principle:** Break the system into pieces that can each be built, tested, and verified independently. Each unit should deliver working software on its own.

## When to Execute

**Execute when:**
- System needs decomposition into multiple units of work
- Multiple services or modules required
- Complex system requiring structured breakdown
- Parallel development across teams/agents needed

**Skip when:**
- Single simple unit
- No decomposition needed
- Straightforward single-component implementation

## Prerequisites

- `application-design` MUST be complete (provides components, methods, services)

## Terminology

- **Unit of Work**: A logical grouping of stories for development. Independently implementable and testable.
- **Service** (microservices): One unit of work per service.
- **Module** (monolith): Logical grouping within the application. Multiple modules may form one unit.
- **Story**: A single user story from `user-stories`. Assigned to exactly one unit.

## Checklist

**Part 1 — Planning:**
1. **Create unit plan** — with checkboxes
2. **Ask clarifying questions** — about grouping, dependencies, organization
3. **Analyze answers** — check for ambiguities
4. **Get plan approval**

<!-- AUDIT: Append to aidd-docs/audit.md:
## Units Generation - Plan Approval
- **Timestamp**: [ISO 8601]
- **User Input**: "[user's response - never summarized]"
- **AI Response**: "[approved/changes requested]"
- **Context**: Unit plan gate

---
-->

**Part 2 — Generation:**
5. **Generate unit definitions** — responsibilities and boundaries
6. **Generate dependency matrix** — which units depend on which
7. **Generate story map** — which stories belong to which unit
8. **Generate code organization** — directory structure (greenfield only)
9. **Get user approval**

<!-- AUDIT: Append to aidd-docs/audit.md:
## Units Generation - Artifacts Approval
- **Timestamp**: [ISO 8601]
- **User Input**: "[user's response - never summarized]"
- **AI Response**: "[approved/changes requested]"
- **Context**: Unit artifacts (definitions, dependencies, story map) complete

---
-->

## Part 1: Planning

### Unit Plan

```markdown
# Units of Work Plan

## Preliminary Units
Based on application design:
- [Unit 1]: [components it covers]
- [Unit 2]: [components it covers]
- [Unit N]: [components it covers]

## Artifacts to Generate
- [ ] unit-of-work.md — unit definitions and responsibilities
- [ ] unit-of-work-dependency.md — dependency matrix
- [ ] unit-of-work-story-map.md — story-to-unit mapping
- [ ] code-organization.md — directory structure (greenfield only)

## Questions
[context-appropriate questions]
```

### Mandatory Question Categories
- **Story grouping**: Which stories naturally belong together?
- **Dependencies**: Which units must be built before others?
- **Technical considerations**: Shared infrastructure, data ownership, API boundaries
- **Business domain**: Do units align with business domains?

### Answer Analysis (MANDATORY)
Check for: vague groupings, circular dependencies, unassigned stories, units that are too large or too small.

## Part 2: Generation

### unit-of-work.md

```markdown
# Units of Work

## Unit 1: [Name]
- **Components**: [from application-design]
- **Stories**: [assigned story IDs]
- **Responsibility**: [one sentence]
- **Boundary**: [what it does NOT include]
- **Dependencies**: [other units it depends on]
- **Build Order**: [sequence position]

## Unit 2: [Name]
...
```

### unit-of-work-dependency.md

```markdown
# Unit Dependencies

## Dependency Matrix
| Unit | Depends On | Must Complete Before |
|------|-----------|---------------------|
| [1]  | —         | [2, 3]              |
| [2]  | [1]       | [4]                 |

## Critical Path
[Unit 1] → [Unit 2] → [Unit 4]
[Unit 1] → [Unit 3]

## Build Order
1. [Unit 1] (no dependencies)
2. [Unit 2] (depends on 1)
3. [Unit 3] (depends on 1, parallel with 2)
4. [Unit 4] (depends on 2)
```

### unit-of-work-story-map.md

```markdown
# Story-to-Unit Mapping

| Story ID | Story Title | Unit | Priority |
|----------|------------|------|----------|
| S001     | [title]    | 1    | Must-have |
| S002     | [title]    | 1    | Must-have |
| S003     | [title]    | 2    | Should-have |

## Coverage Check
- All stories assigned: [Yes/No]
- All units have stories: [Yes/No]
- Unassigned stories: [list any]
```

### code-organization.md (Greenfield Only)

```markdown
# Code Organization

## Directory Structure
[microservices or monolith pattern]

## Rules
- Application code: Workspace root (NEVER in aidd-docs/)
- Each unit maps to: [directory pattern]
- Shared code lives in: [shared location]
```

All artifacts saved to `aidd-docs/inception/units-generation/`.

## State Update

Update `aidd-docs/aidd-state.md`:

```markdown
## Completed Stages
- [x] units-generation — [date]

## Units
- Unit 1: [name]
- Unit 2: [name]
```

## Routing

After user approval:
- Per-unit construction loop begins
- For each unit: `aidd:functional-design` → `aidd:code-generation` (with TDD)
- Or if plans already exist: `aidd:writing-plans` → `aidd:subagent-driven-development`

## Red Flags — STOP

- Generating units without application design (prerequisite)
- Units with circular dependencies
- Stories not assigned to any unit
- Units that are too large (split them)
- Units with no clear boundary
- Proceeding with ambiguous dependency relationships
