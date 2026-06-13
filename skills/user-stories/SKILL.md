---
name: user-stories
description: Use when user-facing features need story-driven planning with acceptance criteria — intelligently skips for internal-only changes
---

# User Stories

Convert requirements into user-centered stories with acceptance criteria.

**Core principle:** Stories capture WHO needs WHAT and WHY. If no user is affected, stories add no value.

## Intelligent Assessment

Before generating stories, assess whether they add value for this project:

### Always Execute (High Priority)
- New user-facing features or functionality
- Changes affecting user workflows or interactions
- Multiple user types or personas involved
- Customer-facing API or service changes
- Complex business requirements with acceptance criteria needs
- Cross-functional team collaboration required

### Execute If Complex (Medium Priority)
Assess: does the change involve multiple components, span user touchpoints, or have significant business impact?
- Backend changes indirectly affecting user experience
- Performance improvements with user-visible benefits
- Integration work impacting user workflows
- Data model changes affecting user data or reports

### Skip (Low Priority)
- Pure internal refactoring with zero user impact
- Simple bug fixes with clear, isolated scope
- Infrastructure changes with no user-facing effects
- Developer tooling or build process improvements
- Documentation-only updates

**Default:** When in doubt, include user stories. The cost of skipping when needed > the cost of including when unnecessary.

## Prerequisites

- `requirements-analysis` complete (recommended)
- `brainstorming` complete

## Checklist

**Part 1 — Planning:**
1. **Validate need** — document the assessment decision
2. **Create story plan** — with checkboxes
3. **Ask clarifying questions** — about user types, workflows, priorities
4. **Analyze answers** — check for ambiguities, follow up
5. **Get plan approval**

<!-- AUDIT: Append to aidd-docs/audit.md:
## User Stories - Plan Approval
- **Timestamp**: [ISO 8601]
- **User Input**: "[user's response - never summarized]"
- **AI Response**: "[approved/changes requested]"
- **Context**: Story plan gate

---
-->

**Part 2 — Generation:**
6. **Generate personas** — user types and their goals
7. **Generate stories** — with INVEST compliance and acceptance criteria
8. **Create persona-story mapping**
9. **Get user approval**

<!-- AUDIT: Append to aidd-docs/audit.md:
## User Stories - Stories Approval
- **Timestamp**: [ISO 8601]
- **User Input**: "[user's response - never summarized]"
- **AI Response**: "[approved/changes requested]"
- **Context**: Stories generation complete

---
-->

## Part 1: Planning

### Story Plan

Create a plan covering:

```markdown
# User Stories Plan

## Assessment
- **Decision**: [include/skip stories]
- **Reason**: [why]

## Story Organization
- **Approach**: [User Journey / Feature / Persona / Domain / Epic-based]
- **Expected Stories**: [estimated count]

## Artifacts to Generate
- [ ] {NNN}-personas.md — user types and goals
- [ ] {NNN}-stories.md — stories with acceptance criteria
- [ ] {NNN}-story-map.md — persona-to-story mapping
- [ ] INVEST compliance check

## Questions
[context-appropriate questions with [Answer]: tags]
```

### Mandatory Question Categories
- Who are the users? (personas, roles, skill levels)
- What are the primary user journeys?
- What does success look like for each user type?
- Are there priority stories (must-have vs nice-to-have)?

### Answer Analysis (MANDATORY)
After collecting answers, check for:
- Vague terms → ask for specifics
- Undefined personas → define them before proceeding
- Contradictory priorities → resolve with user
- Missing user types → confirm completeness

## Part 2: Generation

Read **Next Doc ID** from `aidd-state.md` under `## Document Counter`. Use this number as the filename prefix for each file below. After creating each file, increment the counter.

### Personas

Create `aidd-docs/inception/user-stories/{NNN}-personas.md`:

```markdown
# User Personas

## [Persona Name]
- **Role**: [their role]
- **Goals**: [what they want to accomplish]
- **Technical Level**: [novice/intermediate/expert]
- **Key Scenarios**: [main use cases]
```

### Stories

Create `aidd-docs/inception/user-stories/{NNN}-stories.md`:

```markdown
# User Stories

## Story 1: [Title]
**As a** [persona], **I want to** [action] **so that** [benefit].

### Acceptance Criteria
- [ ] [criterion 1]
- [ ] [criterion 2]
- [ ] [criterion 3]

### Priority: [Must-have / Should-have / Nice-to-have]
### Story Points: [estimate]
```

### INVEST Compliance Check

Each story should be:
- **I**ndependent — can be developed alone
- **N**egotiable — details can be discussed
- **V**aluable — delivers user value
- **E**stimable — can be sized
- **S**mall — fits in a sprint/iteration
- **T**estable — has clear acceptance criteria

Flag any stories that violate INVEST and suggest splitting or rewording.

### Story Map

Create `aidd-docs/inception/user-stories/{NNN}-story-map.md` — mapping personas to their stories for traceability.

## State Update

After completion, update `aidd-docs/aidd-state.md`:

```markdown
## Completed Stages
- [x] user-stories — [date]
```

## Routing

After user approval:
- Complex system with multiple components → `aidd:application-design`
- Clear scope, few components → `aidd:writing-plans`
- System needs decomposition → `aidd:units-generation`

## Red Flags — STOP

- Generating stories for pure refactoring (no user impact)
- Stories without acceptance criteria
- Stories that aren't testable
- Skipping the assessment (always evaluate first)
- Proceeding with ambiguous persona definitions
- Stories that are too large (split them)
