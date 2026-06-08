# Resiliency Baseline Rules

Cross-cutting resiliency constraints enforced across all AIDD phases when enabled.

## Blocking Behavior

When a resiliency rule violation is found:
1. List the finding in the stage completion message
2. Stage CANNOT proceed until finding is resolved
3. Present only "Request Changes" option until fixed

## Rules

### RESILIENCY-01: Critical Workload Identification
- Identify and classify critical workloads
- Document impact analysis for each workload
- Map dependencies between workloads

### RESILIENCY-02: Availability & Recovery Targets
- Define SLA targets for each service
- Specify RTO (Recovery Time Objective)
- Specify RPO (Recovery Point Objective)
- Align targets with business requirements

### RESILIENCY-03: Fault Tolerance
- Implement retry logic with exponential backoff
- Use circuit breakers for external dependencies
- Provide fallback responses where possible
- Handle partial failures gracefully

### RESILIENCY-04: Health Checks
- Implement health check endpoints
- Include dependency health in checks
- Use health checks for load balancer routing

### RESILIENCY-05: Graceful Degradation
- Define behavior when dependencies are unavailable
- Maintain core functionality during partial outages
- Communicate degraded state to users

### RESILIENCY-06: Observability
- Implement structured logging for all error paths
- Add correlation IDs across service boundaries
- Monitor key business metrics, not just technical metrics
