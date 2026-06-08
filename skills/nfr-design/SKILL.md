---
name: nfr-design
description: Use when non-functional requirements need analysis and design — performance, scalability, security, and tech stack decisions with design patterns
---

# NFR Design

Analyze non-functional requirements and incorporate them into the unit design with appropriate patterns.

**Core principle:** NFRs are not afterthoughts. Performance, security, and scalability decisions made during design are cheaper than retrofitting them after implementation.

**This skill merges NFR requirements gathering AND NFR design** into a single workflow — determine what you need, then design how to achieve it.

## When to Execute

**Execute when:**
- Performance requirements exist (latency, throughput targets)
- Security considerations needed (compliance, data protection)
- Scalability concerns present (load growth, capacity planning)
- Tech stack selection required
- High availability or disaster recovery requirements

**Skip when:**
- No NFR requirements beyond defaults
- Tech stack already determined and locked
- Simple changes with no quality attribute concerns

## Prerequisites

- `functional-design` complete for the unit

## Checklist

1. **Analyze functional design** — understand business logic complexity
2. **Determine NFR scope** — which quality attributes matter
3. **Ask clarifying questions** — about scalability, performance, security, tech stack
4. **Analyze answers** — check for ambiguities, follow up
5. **Generate NFR requirements** — document targets and constraints
6. **Generate NFR design patterns** — select patterns to meet requirements
7. **Make tech stack decisions** — with rationale
8. **Get user approval**

## Step 1: Analyze Functional Design

Read functional design artifacts from `aidd-docs/construction/{unit-name}/functional-design/`. Understand the business logic complexity to assess which NFR categories are relevant.

## Step 2: Determine NFR Scope

Not every project needs every NFR category. Assess relevance:

| Category | Evaluate When |
|----------|---------------|
| **Scalability** | Expected load > single instance, growth projections |
| **Performance** | Latency-sensitive operations, throughput requirements |
| **Availability** | Uptime SLA requirements, business impact of downtime |
| **Security** | PII/sensitive data, compliance requirements, external APIs |
| **Tech Stack** | New project, or existing stack can't meet requirements |
| **Reliability** | Error rate requirements, fault tolerance needed |

## Step 3: Clarifying Questions

Ask about relevant NFR categories. **When in doubt, ask.**

**Scalability:**
- Expected concurrent users / requests per second?
- Growth projections over 6/12/24 months?
- Scaling strategy: vertical, horizontal, auto-scaling?

**Performance:**
- Target response times (p50, p95, p99)?
- Throughput requirements?
- Any operations with known performance constraints?

**Availability:**
- Uptime target (99.9%, 99.99%)?
- Acceptable downtime window?
- Disaster recovery requirements (RTO/RPO)?

**Security:**
- What data is sensitive (PII, credentials, financial)?
- Compliance requirements (SOC2, HIPAA, GDPR)?
- Authentication/authorization model?

**Tech stack:**
- Technology preferences or constraints?
- Existing systems to integrate with?
- Team expertise considerations?

Analyze ALL answers for ambiguities. Follow up on vague targets ("fast", "reliable", "secure") with specific numbers.

## Step 4: Generate Artifacts

Save to `aidd-docs/construction/{unit-name}/nfr-design/`:

### nfr-requirements.md
```markdown
# NFR Requirements — [unit-name]

## Performance
- **Response Time**: [p50/p95/p99 targets]
- **Throughput**: [requests/second or transactions/second]

## Scalability
- **Current Load**: [baseline]
- **Growth Target**: [projected load]
- **Scaling Strategy**: [horizontal/vertical/auto]

## Availability
- **Uptime Target**: [SLA percentage]
- **RTO**: [recovery time objective]
- **RPO**: [recovery point objective]

## Security
- **Data Classification**: [what's sensitive]
- **Compliance**: [regulatory requirements]
- **Auth Model**: [how users authenticate]

## Reliability
- **Error Budget**: [acceptable error rate]
- **Fault Tolerance**: [what failures to handle]
```

### tech-stack-decisions.md
```markdown
# Tech Stack Decisions — [unit-name]

## Decisions
### [Category: Language/Framework/Database/etc.]
- **Choice**: [selected technology]
- **Rationale**: [why this choice]
- **Alternatives Considered**: [what else was evaluated]
- **Constraints**: [limitations or trade-offs]
```

### nfr-design-patterns.md
```markdown
# NFR Design Patterns — [unit-name]

## Patterns Applied
### [Pattern Name: e.g., Circuit Breaker, Retry, Caching]
- **Addresses**: [which NFR requirement]
- **Where**: [which component/operation]
- **Configuration**: [parameters]
- **Fallback**: [what happens when pattern activates]

## Logical Components
### [Component: e.g., Cache, Queue, Load Balancer]
- **Purpose**: [why it exists]
- **Technology**: [specific implementation]
- **Integration**: [how it connects to other components]
```

## State Update

Update `aidd-docs/aidd-state.md`:

```markdown
## Completed Stages
- [x] nfr-design — [unit-name] — [date]
```

## Routing

After user approval:
- Infrastructure mapping needed → `aidd:infrastructure-design`
- Ready to implement → `aidd:code-generation`

## Red Flags — STOP

- Treating NFRs as optional ("we'll optimize later")
- Vague performance targets without numbers
- Skipping security analysis for "internal" systems
- Choosing tech stack without understanding constraints
- Not considering failure modes
- Proceeding with contradictory requirements (e.g., "instant response" + "heavy computation")
