# Skill Authoring Guide

Quick reference for creating new AIDD skills or editing existing ones.

## Skill Structure

```
skills/
  skill-name/
    SKILL.md              # Main reference (required)
    supporting-file.*     # Only if needed
```

## Frontmatter (Required)

```yaml
---
name: skill-name-in-kebab-case
description: Use when [specific triggering conditions]
---
```

- `name`: lowercase kebab-case, matches directory name
- `description`: starts with "Use when...", describes when to invoke
- Max description length: 1024 characters

## Size Targets

- **<500 lines** hard maximum
- **<200 lines** preferred for frequently loaded skills
- Supporting files for detailed reference material

## Skill Types

| Type | Description | Example |
|------|-------------|---------|
| **Discipline** | Enforces a rule (TDD, verification) | test-driven-development |
| **Technique** | Concrete method with steps | systematic-debugging |
| **Pattern** | Way of thinking about problems | brainstorming |
| **Reference** | API docs, syntax guides | anthropic-best-practices |

## Writing Rules

1. **No placeholders**: No "TBD", "TODO", "fill in details"
2. **Anti-patterns**: Include red flags / rationalization table
3. **Self-contained**: Everything the agent needs in one load
4. **Actionable**: "Do X" not "Consider X"
5. **Cross-reference**: Use `aidd:skill-name` format for references

## Cross-References

Use the `aidd:` prefix when referencing other skills:

```markdown
- **Required skill:** Use `aidd:test-driven-development`
- Next skill: `aidd:code-generation`
```

## Testing New Skills

Before deploying a new skill:
1. Invoke it from a relevant scenario
2. Verify the agent follows the skill's process
3. Check that anti-patterns are caught
4. Confirm cross-references resolve correctly
