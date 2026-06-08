---
name: application-design
description: Use when new components, services, or dependencies need identifying — high-level component design with interfaces and dependency mapping
---

# Application Design

High-level component identification and service layer design.

**Core principle:** Identify components and their boundaries before diving into implementation. Each component should have one clear responsibility and well-defined interfaces.

**Focus:** Components, responsibilities, interfaces, services, dependencies. NOT detailed business logic (that's `functional-design`).

## When to Execute

**Execute when:**
- New components or services needed
- Component methods and business rules need definition
- Service layer design required
- Component dependencies need clarification

**Skip when:**
- Changes within existing component boundaries
- No new components or methods
- Pure implementation changes with clear structure

## Prerequisites

- `requirements-analysis` complete
- `user-stories` complete (if applicable)

## Checklist

1. **Analyze context** — read requirements and user stories
2. **Create design plan** — with checkboxes
3. **Ask clarifying questions** — about components, interfaces, patterns
4. **Analyze answers** — check for ambiguities
5. **Generate design artifacts** — components, methods, services, dependencies
6. **Get user approval**
7. **Route to next skill**

## Step 1: Analyze Context

Read and understand:
- Requirements from `aidd-docs/inception/requirements/`
- User stories from `aidd-docs/inception/user-stories/` (if generated)
- Reverse engineering artifacts (if brownfield)

## Step 2: Design Plan

Create a plan identifying:

```markdown
# Application Design Plan

## Components to Define
- [ ] [component 1]
- [ ] [component 2]

## Artifacts to Generate
- [ ] components.md — definitions and responsibilities
- [ ] component-methods.md — method signatures
- [ ] services.md — service definitions and orchestration
- [ ] component-dependency.md — dependency matrix

## Questions
[context-appropriate questions]
```

## Step 3: Clarifying Questions

Mandatory question categories:
- **Component identification**: What are the main functional areas?
- **Component methods**: What operations does each component expose?
- **Service layer**: How do components orchestrate together?
- **Dependencies**: What depends on what? What's the communication pattern?
- **Design patterns**: Any patterns to follow (event-driven, CQRS, etc.)?

Prefer multiple choice. One question at a time. Analyze ALL answers for ambiguities before proceeding.

## Step 4: Generate Artifacts

### components.md

```markdown
# Components

## [Component Name]
- **Purpose**: [one sentence]
- **Responsibilities**: [bulleted list]
- **Interfaces**: [what it exposes to other components]
- **Boundaries**: [what it does NOT do]
```

### component-methods.md

```markdown
# Component Methods

## [Component Name]
### [MethodName](params): ReturnType
- **Purpose**: [one sentence]
- **Input**: [types]
- **Output**: [types]
- **Notes**: [side effects, constraints]

(Detailed business rules are defined in functional-design, not here)
```

### services.md

```markdown
# Services

## [Service Name]
- **Purpose**: [one sentence]
- **Orchestrates**: [which components it coordinates]
- **API**: [exposed endpoints/operations]
- **Interactions**: [how it calls components]
```

### component-dependency.md

```markdown
# Component Dependencies

## Dependency Matrix
| Component | Depends On | Communicates Via |
|-----------|-----------|-----------------|
| [A]       | [B, C]    | [events, API]   |

## Communication Patterns
- [pattern descriptions]
```

All artifacts saved to `aidd-docs/inception/application-design/`.

## State Update

Update `aidd-docs/aidd-state.md`:

```markdown
## Completed Stages
- [x] application-design — [date]
```

## Routing

After user approval:
- Complex system needing decomposition → `aidd:units-generation`
- Clear scope, ready to implement → `aidd:writing-plans`

## Red Flags — STOP

- Designing detailed business logic (that's functional-design)
- Creating components without clear responsibilities
- Undefined component boundaries
- Circular dependencies between components
- Skipping dependency analysis
- Proceeding with ambiguous interface definitions
