---
name: requirements-analysis
description: Use when requirements need gathering or validation after brainstorming — adaptive depth from minimal to comprehensive, with overconfidence prevention
---

# Requirements Analysis

Gather and validate requirements with depth that matches the problem's complexity.

**Core principle:** When in doubt, ask. Overconfidence leads to poor implementations.

## Adaptive Depth

Always executes, but detail level adapts to the request:

- **Minimal** — Clear, simple request. Document intent analysis only. Skip questions if requirements are unambiguous.
- **Standard** — Normal feature. Gather functional and non-functional requirements with clarifying questions.
- **Comprehensive** — Complex system, high risk. Detailed requirements with traceability, extensive analysis.

Assess depth based on: request clarity, complexity, scope, risk level, stakeholder count.

## Prerequisites

- `workspace-detection` complete
- `brainstorming` complete (or clear user request for simple cases)

## Checklist

1. **Analyze user request** — determine clarity, type, scope, complexity
2. **Determine depth** — minimal / standard / comprehensive
3. **Assess completeness** — evaluate all requirement areas
4. **Ask clarifying questions** — for ANY ambiguity (standard+ depth)
5. **Present extension opt-ins** — security, resiliency, testing (standard+ depth)
6. **Generate requirements document** — with intent analysis summary
7. **Update state file** — mark requirements analysis complete
8. **Route to next skill** — based on project type

## Step 1: Intent Analysis

Analyze the user's request across four dimensions:

**Request clarity:**
- Clear: Specific, well-defined, actionable
- Vague: General, ambiguous, needs clarification
- Incomplete: Missing key information

**Request type:**
- New feature / Bug fix / Refactoring / Migration / Enhancement / New project

**Scope estimate:**
- Single file / Single component / Multi-component / System-wide / Cross-system

**Complexity estimate:**
- Trivial / Simple / Moderate / Complex

## Step 2: Determine Requirements Depth

**Minimal** — use when:
- Request is clear and simple
- No detailed requirements needed
- Just document the basic understanding

**Standard** — use when:
- Request needs clarification
- Functional and non-functional requirements needed
- Normal complexity

**Comprehensive** — use when:
- Complex project with multiple stakeholders
- High risk or critical system
- Detailed requirements with traceability needed

## Step 3: Completeness Analysis

**MANDATORY for standard+ depth.** Evaluate ALL areas:

- **Functional**: Core features, user interactions, system behaviors
- **Non-functional**: Performance, security, scalability, usability
- **User scenarios**: Use cases, user journeys, edge cases, error scenarios
- **Business context**: Goals, constraints, success criteria, stakeholder needs
- **Technical context**: Integration points, data requirements, system boundaries
- **Quality attributes**: Reliability, maintainability, testability, accessibility

Ask questions for ANY area that is unclear. Default to asking rather than assuming.

## Step 4: Clarifying Questions (Standard+ Depth)

Ask questions one at a time, focusing on gaps from the completeness analysis. Prefer multiple choice when possible, but open-ended is fine.

**Overconfidence prevention:** If ANY ambiguity exists, ask the question. "I think I understand" is not sufficient — confirm.

Follow-up is MANDATORY for:
- Vague terms ("good performance" → what latency target?)
- Undefined criteria ("it should work" → what does success look like?)
- Contradictory responses
- Missing details
- Answers that combine multiple options

## Step 5: Extension Opt-Ins (Standard+ Depth)

If extensions exist in the project, present opt-in questions:
- Security baseline
- Resiliency baseline
- Property-based testing

Record user's choices in `aidd-docs/aidd-state.md` under Extension Configuration.

For each extension opted IN: load the full rules file.
For each extension opted OUT: do NOT load the rules file.

## Step 6: Generate Requirements Document

Create `aidd-docs/inception/requirements/requirements.md` containing:

```markdown
# Requirements

## Intent Analysis
- **Request**: [user's original request]
- **Type**: [feature/bugfix/etc.]
- **Scope**: [scope estimate]
- **Complexity**: [complexity estimate]
- **Depth**: [minimal/standard/comprehensive]

## Functional Requirements
- [requirements]

## Non-Functional Requirements
- [requirements if applicable]

## Success Criteria
- [measurable outcomes]

## Assumptions
- [assumptions made, if any]

## Extension Configuration
- [enabled/disabled extensions]
```

## Step 7: Update State

Update `aidd-docs/aidd-state.md`:

```markdown
## Completed Stages
- [x] requirements-analysis — [date]
```

## Step 8: Present and Route

Present requirements summary to user:

```
📋 Requirements Analysis Complete

• **Depth**: [minimal/standard/comprehensive]
• **Scope**: [scope]
• **Key Requirements**: [bullet list]

Review the requirements at: aidd-docs/inception/requirements/requirements.md

🔧 Request Changes
✅ Approve & Continue → [next skill]
```

**Routing:**
- User-facing feature with complexity → `aidd:user-stories`
- New components/services needed → `aidd:application-design`
- Simple change, clear scope → skip directly to `aidd:writing-plans`

## Red Flags — STOP

- Assuming requirements instead of asking
- "I think they mean..." — ASK
- Skipping completeness analysis for "simple" requests
- Not asking follow-up questions for vague answers
- Proceeding with ANY unresolved ambiguity
- Writing "TBD" or "TODO" in requirements
