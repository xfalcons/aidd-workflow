---
name: reverse-engineering
description: Use when a brownfield codebase is detected — analyzes existing code to generate architecture, API, and component documentation
---

# Reverse Engineering

Analyze an existing codebase and generate comprehensive design artifacts.

**Core principle:** Understand what exists before deciding what to change. Existing code is the source of truth.

## When to Execute

**Execute when:** Brownfield project (existing code found in workspace), no current reverse engineering artifacts.

**Skip when:** Greenfield project (no existing code).

**Rerun when:** Artifacts are stale (older than last significant code modification) or user explicitly requests it.

## Prerequisites

- `workspace-detection` complete, detected brownfield

## Checklist

1. **Multi-package discovery** — scan workspace for all packages and components
2. **Business context analysis** — understand business transactions and purpose
3. **Architecture documentation** — generate system architecture with diagrams
4. **Code structure documentation** — map files, classes, patterns
5. **API documentation** — document all APIs and data models
6. **Component inventory** — catalog all packages by type
7. **Technology stack** — document languages, frameworks, tools
8. **Present findings** — get user approval before proceeding

## Step 1: Multi-Package Discovery

Scan the workspace (NOT inside `aidd-docs/`) for:

- **All packages**: Check for package.json, pom.xml, build.gradle, Cargo.toml, go.mod, etc.
- **Package relationships**: Config files showing dependencies between packages
- **Package types**: Application, Infrastructure (CDK/Terraform/CloudFormation), Models, Clients, Tests
- **Build systems**: Maven, Gradle, npm, Brazil, Cargo, etc.
- **Service architecture**: Lambda functions, containers, APIs, data stores
- **Code quality**: Languages, frameworks, test coverage indicators, linting, CI/CD

## Step 2: Business Context Analysis

Understand the business the system implements:

- What is the core business purpose of this system?
- What business transactions does it implement?
- How do packages/components relate to business domains?

## Step 3: Generate Artifacts

Read **Next Doc ID** from `aidd-state.md` under `## Document Counter`. Use this number as the filename prefix for each file below. After creating each file, increment the counter in `aidd-state.md`.

Create the following in `aidd-docs/inception/reverse-engineering/`:

### {NNN}-architecture.md
```markdown
# System Architecture

## Overview
[High-level system description]

## Architecture Diagram
[Mermaid diagram showing packages, services, data stores, relationships]

## Component Descriptions
### [Component Name]
- **Purpose**: [what it does]
- **Dependencies**: [what it depends on]
- **Type**: [Application/Infrastructure/Model/Client]

## Data Flow
[Key workflow diagrams]

## Integration Points
- External APIs, databases, third-party services
```

### {NNN}-code-structure.md
```markdown
# Code Structure

## Build System
- **Type**: [Maven/npm/Cargo/etc.]
- **Configuration**: [key build files]

## Key Files Inventory
- `path/to/file` — [purpose/responsibility]

## Design Patterns
### [Pattern Name]
- **Location**: [where used]
- **Purpose**: [why used]
```

### {NNN}-api-documentation.md
```markdown
# API Documentation

## REST APIs
### [Endpoint]
- **Method**: [GET/POST/PUT/DELETE]
- **Path**: [/api/path]
- **Purpose**: [what it does]
- **Request/Response**: [formats]

## Data Models
### [Model Name]
- **Fields**: [descriptions]
- **Relationships**: [related models]
```

### {NNN}-component-inventory.md
```markdown
# Component Inventory

## Application Packages
- [name] — [purpose]

## Infrastructure Packages
- [name] — [CDK/Terraform] — [purpose]

## Total Count
- **Total Packages**: [number]
- **Application**: [number] | **Infrastructure**: [number]
```

### {NNN}-technology-stack.md
```markdown
# Technology Stack

## Languages
- [language] — [version] — [usage]

## Frameworks
- [framework] — [version] — [purpose]

## Infrastructure & Tools
- [service/tool] — [purpose]
```

## Step 4: Update State

Update `aidd-docs/aidd-state.md`:

```markdown
## Completed Stages
- [x] reverse-engineering — [date]

## Reverse Engineering Artifacts
- Location: aidd-docs/inception/reverse-engineering/
- Date: [ISO timestamp]
```

## Step 5: Present and Get Approval

<!-- AUDIT: Append to aidd-docs/audit.md before asking for approval:
## Reverse Engineering - Approval Prompt
- **Timestamp**: [ISO 8601]
- **User Input**: N/A (prompt)
- **AI Response**: "Presenting findings for approval"
- **Context**: Reverse engineering complete

---
-->

```
🔍 Reverse Engineering Complete

[Summary of key findings]

Review artifacts at: aidd-docs/inception/reverse-engineering/

🔧 Request Changes
✅ Approve & Continue → brainstorming
```

Wait for explicit user approval before proceeding to `aidd:brainstorming`.

<!-- AUDIT: Append to aidd-docs/audit.md after receiving user response:
## Reverse Engineering - Approval Response
- **Timestamp**: [ISO 8601]
- **User Input**: "[user's complete response - never summarized]"
- **AI Response**: "[approved/changes requested]"
- **Context**: User approval decision

---
-->

## Key Principles

- **Exhaustive scanning**: Check ALL packages, not just the ones mentioned
- **Business-first**: Understand the business before documenting the tech
- **Source of truth**: Existing code is always right — document what IS, not what should be
- **Staleness detection**: Re-run if code has changed since last analysis

## Red Flags — STOP

- Skipping packages because they "look unimportant"
- Documenting what code SHOULD do instead of what it DOES
- Proceeding without understanding the business context
- Creating artifacts without verifying against actual code
- Missing integration points between components
