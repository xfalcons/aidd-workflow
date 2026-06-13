---
name: functional-design
description: Use when a unit needs detailed business logic design — domain models, business rules, and validation logic before code generation
---

# Functional Design

Detailed business logic design per unit of work.

**Core principle:** Design the business logic before choosing the technology. Focus on WHAT the system does, not HOW it's implemented.

**Focus:** Business logic, domain models, business rules, validation logic. NOT infrastructure or deployment (that's `infrastructure-design`).

## When to Execute

**Execute when:**
- New data models or schemas needed
- Complex business logic to implement
- Business rules need detailed design before coding

**Skip when:**
- Simple logic changes
- No new business logic
- Direct implementation from stories is sufficient

## Prerequisites

- `units-generation` complete (or single-unit project)
- Unit of work artifacts available
- `application-design` recommended (provides high-level component structure)

## Checklist

1. **Analyze unit context** — read unit definition and assigned stories
2. **Create design plan** — with checkboxes
3. **Ask clarifying questions** — about business logic, domain, rules
4. **Analyze answers** — check for ambiguities, follow up
5. **Generate design artifacts** — business logic, rules, domain entities
6. **Get user approval**

<!-- AUDIT: Append to aidd-docs/audit.md:
## Functional Design - Approval
- **Timestamp**: [ISO 8601]
- **User Input**: "[user's response - never summarized]"
- **AI Response**: "[approved/changes requested]"
- **Context**: Functional design [unit-name] complete

---
-->

## Step 1: Analyze Unit Context

Read and understand:
- Unit definition from `aidd-docs/inception/units-generation/` (filenames are serial-number prefixed, e.g., `001-unit-of-work.md`)
- Assigned stories from `aidd-docs/inception/units-generation/` (e.g., `001-unit-of-work-story-map.md`)
- Application design from `aidd-docs/inception/application-design/` (if exists, filenames are serial-number prefixed)
- Reverse engineering artifacts (if brownfield)

## Step 2: Create Design Plan

```markdown
# Functional Design Plan — [unit-name]

## Scope
- Stories: [assigned story IDs]
- Components: [from application-design]

## Artifacts to Generate
- [ ] {NNN}-business-logic-model.md
- [ ] {NNN}-business-rules.md
- [ ] {NNN}-domain-entities.md
- [ ] {NNN}-frontend-components.md (if applicable)

## Questions
[context-appropriate questions with [Answer]: tags]
```

## Step 3: Clarifying Questions

**Mandatory categories to evaluate** (ask about ALL that apply):

- **Business logic**: Core workflows, data transformations, business processes
- **Domain model**: Entities, relationships, data structures, business objects
- **Business rules**: Decision rules, validation logic, constraints, policies
- **Data flow**: Inputs, outputs, transformations, persistence requirements
- **Integration points**: External systems, APIs, data exchange
- **Error handling**: Error scenarios, validation failures, exception handling
- **Edge cases**: Alternative flows, complex business situations
- **Frontend** (if applicable): Component structure, interactions, state, forms

**Overconfidence prevention:** Default to asking. "I think I understand" is not sufficient — confirm.

Analyze ALL answers for ambiguities. Follow up on vague, undefined, or contradictory responses before proceeding.

## Step 4: Generate Artifacts

Read **Next Doc ID** from `aidd-state.md` under `## Document Counter`. Use this number as the filename prefix for each file below. After creating each file, increment the counter.

Save to `aidd-docs/construction/{unit-name}/functional-design/`:

### {NNN}-business-logic-model.md
```markdown
# Business Logic — [unit-name]

## Core Workflows
### [Workflow Name]
- **Trigger**: [what starts it]
- **Steps**: [ordered list of business operations]
- **Outcome**: [what it produces]
- **Edge Cases**: [alternative flows]

## Data Transformations
### [Transformation Name]
- **Input**: [what goes in]
- **Output**: [what comes out]
- **Rules**: [transformation logic]
```

### {NNN}-business-rules.md
```markdown
# Business Rules — [unit-name]

## Rule 1: [Name]
- **Condition**: [when this rule applies]
- **Action**: [what happens]
- **Constraint**: [limits or boundaries]
- **Validation**: [how to verify compliance]

## Validation Rules
### [Entity] Validation
- [field]: [constraint] → [error message]
```

### {NNN}-domain-entities.md
```markdown
# Domain Entities — [unit-name]

## [Entity Name]
- **Purpose**: [one sentence]
- **Properties**:
  - [property]: [type] — [description]
- **Relationships**: [related entities]
- **Invariants**: [business constraints that must always hold]
```

### {NNN}-frontend-components.md (if applicable)
```markdown
# Frontend Components — [unit-name]

## [Component Name]
- **Purpose**: [what it renders/does]
- **Props**: [input properties]
- **State**: [internal state]
- **User Interactions**: [click handlers, form submissions]
- **API Calls**: [backend endpoints used]
```

## State Update

Update `aidd-docs/aidd-state.md`:

```markdown
## Completed Stages
- [x] functional-design — [unit-name] — [date]
```

## Routing

After user approval:
- NFR concerns exist → `aidd:nfr-design`
- Infrastructure mapping needed → `aidd:infrastructure-design`
- Ready to implement → `aidd:code-generation`

## Red Flags — STOP

- Designing infrastructure (that's infrastructure-design)
- Skipping business rules because they "seem obvious"
- Proceeding with ambiguous domain entity definitions
- Not defining validation rules for user inputs
- Missing edge cases in business workflows
