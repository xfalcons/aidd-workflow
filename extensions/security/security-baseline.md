# Security Baseline Rules

Cross-cutting security constraints enforced across all AIDD phases when enabled.

## Blocking Behavior

When a security rule violation is found:
1. List the finding in the stage completion message
2. Stage CANNOT proceed until finding is resolved
3. Present only "Request Changes" option until fixed

## Rules

### SECURITY-01: Encryption
- Encrypt data at rest and in transit
- Use managed key services where available
- Enforce TLS 1.2+ for all network communication

### SECURITY-02: Access Logging
- Enable access logging on all network intermediaries (load balancers, API gateways, CDN)
- Log all access events with timestamp, source, and action

### SECURITY-03: Application Logging
- Use centralized log service
- Structured logging format
- Never log secrets, credentials, or PII in plaintext

### SECURITY-04: HTTP Security Headers
- Content-Security-Policy
- Strict-Transport-Security
- X-Content-Type-Options: nosniff
- X-Frame-Options
- Referrer-Policy

### SECURITY-05: Input Validation
- Validate ALL API parameters: type, length, format
- Sanitize all user inputs
- Prevent injection attacks (SQL, XSS, command)

### SECURITY-06: Secrets Management
- Never hardcode secrets in code or config
- Use environment variables or secret managers
- Rotate secrets regularly

### SECURITY-07: Authentication & Authorization
- No default credentials
- Least privilege access
- Multi-factor authentication for critical systems
