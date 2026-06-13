---
name: infrastructure-design
description: Use when logical software components need mapping to actual cloud infrastructure — deployment architecture, compute, storage, and networking decisions
---

# Infrastructure Design

Map logical software components to actual infrastructure choices for deployment.

**Core principle:** Infrastructure serves the application, not the other way around. Start from what the application needs, then choose the infrastructure that meets those needs.

## When to Execute

**Execute when:**
- Infrastructure services need mapping (compute, storage, networking)
- Deployment architecture required (cloud, hybrid, on-premise)
- Cloud resources need specification
- Shared infrastructure across units needs planning

**Skip when:**
- No infrastructure changes
- Infrastructure already defined and current
- Serverless/simple deployment with no architecture decisions

## Prerequisites

- `functional-design` complete for the unit
- `nfr-design` recommended (provides logical components and patterns to map)

## Checklist

1. **Analyze design artifacts** — read functional + NFR design
2. **Create infrastructure plan** — with checkboxes
3. **Ask clarifying questions** — about deployment, compute, storage, networking
4. **Analyze answers** — check for ambiguities
5. **Generate infrastructure artifacts** — mapping, deployment architecture, shared infra
6. **Get user approval**

<!-- AUDIT: Append to aidd-docs/audit.md:
## Infrastructure Design - Approval
- **Timestamp**: [ISO 8601]
- **User Input**: "[user's response - never summarized]"
- **AI Response**: "[approved/changes requested]"
- **Context**: Infrastructure design [unit-name] complete

---
-->

## Step 1: Analyze Design Artifacts

Read and understand:
- Functional design from `aidd-docs/construction/{unit-name}/functional-design/`
- NFR design from `aidd-docs/construction/{unit-name}/nfr-design/` (if exists)
- Logical components identified in NFR design that need infrastructure mapping
- Reverse engineering infrastructure artifacts (if brownfield)

## Step 2: Clarifying Questions

**Mandatory categories to evaluate** (ask about ALL that apply):

- **Deployment environment**: Cloud provider preference? Region(s)? Environment strategy (dev/staging/prod)?
- **Compute**: Serverless (Lambda/Functions) vs containers (ECS/EKS) vs VMs? Sizing? Scaling configuration?
- **Storage**: Database type (relational, document, key-value)? Caching strategy? Data lifecycle?
- **Messaging**: Queue/event requirements? Async processing? Event-driven patterns?
- **Networking**: Load balancing? API gateway? VPC/network topology? CDN?
- **Monitoring**: Observability tooling? Alerting strategy? Log aggregation?
- **Shared infrastructure**: Multi-unit sharing? Multi-tenancy? Resource isolation?

**Overconfidence prevention:** If unsure about infrastructure options, present 2-3 alternatives with trade-offs.

## Step 3: Generate Artifacts

Save to `aidd-docs/construction/{unit-name}/infrastructure-design/`:

### infrastructure-design.md
```markdown
# Infrastructure Design — [unit-name]

## Compute
### [Service: e.g., Lambda, ECS, EC2]
- **Purpose**: [what runs here]
- **Configuration**: [memory/CPU/concurrency/etc.]
- **Scaling**: [auto-scaling rules]
- **Rationale**: [why this choice]

## Storage
### [Service: e.g., DynamoDB, RDS, S3]
- **Purpose**: [what data it holds]
- **Configuration**: [capacity, replication, backup]
- **Access Pattern**: [read/write patterns]

## Messaging
### [Service: e.g., SQS, EventBridge, SNS]
- **Purpose**: [what flows through it]
- **Configuration**: [queue depth, retry policy, DLQ]

## Networking
### [Service: e.g., ALB, API Gateway, CloudFront]
- **Purpose**: [routing/CDN/load balancing]
- **Configuration**: [rules, health checks, TLS]
```

### deployment-architecture.md
```markdown
# Deployment Architecture — [unit-name]

## Architecture Diagram
[Mermaid diagram showing deployment topology]

## Environments
### Production
- **Region**: [region]
- **Services**: [list]
- **Networking**: [VPC, subnets]

### Staging
- [mirrors production or differences]

## Deployment Strategy
- **Method**: [blue/green, canary, rolling]
- **CI/CD**: [pipeline approach]
- **Rollback**: [how to roll back]

## Resource Summary
| Resource | Service | Purpose | Environment |
|----------|---------|---------|-------------|
| [name]   | [type]  | [why]   | [where]     |
```

### shared-infrastructure.md (if applicable)
```markdown
# Shared Infrastructure

## Shared Resources
- [resources used across multiple units]

## Resource Isolation
- [how units are isolated from each other]

## Ownership
- [which team/unit owns which shared resources]
```

## State Update

Update `aidd-docs/aidd-state.md`:

```markdown
## Completed Stages
- [x] infrastructure-design — [unit-name] — [date]
```

## Routing

After user approval:
- Ready to implement → `aidd:code-generation`
- More units to design → `aidd:functional-design` for next unit

## Red Flags — STOP

- Choosing infrastructure before understanding application needs
- Over-provisioning "just in case" (start minimal, scale based on NFR targets)
- Not considering cost implications of infrastructure choices
- Skipping networking/security groups configuration
- No rollback strategy for deployments
- Proceeding without understanding existing infrastructure (brownfield)
