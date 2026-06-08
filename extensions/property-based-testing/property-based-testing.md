# Property-Based Testing Rules

Cross-cutting PBT constraints enforced during construction when enabled.

## Blocking Behavior

When a PBT rule violation is found during code generation:
1. List the finding in the completion message
2. Stage CANNOT proceed until finding is resolved
3. Present only "Request Changes" option until fixed

## Rules

### PBT-01: Property Identification During Design
- During functional design, identify properties that can be tested
- Property types: round-trip, invariant, idempotence, commutativity, oracle, induction
- Document properties as part of the design artifacts

### PBT-02: Round-Trip Properties
- Serialization: serialize then deserialize produces original
- Encoding: encode then decode produces original
- Encryption: encrypt then decrypt produces original
- Database: write then read produces original

### PBT-03: Invariant Properties
- Size preservation (e.g., filter doesn't add elements)
- Element preservation (e.g., sort contains same elements)
- Ordering guarantees (e.g., sorted output is ordered)
- Range constraints (e.g., values stay within bounds)
- Type preservation (e.g., transform output type matches expected)

### PBT-04: Idempotency Properties
- Verify f(f(x)) = f(x) where applicable
- Apply operations twice, ensure same result as once
- Relevant for: PUT endpoints, configuration updates, state transitions

### PBT-05: Commutativity Properties
- Verify different orderings produce same result where applicable
- Example: set union, map merge (for commutative operations)

### PBT-06: Exemptions
- Pure configuration code does not require PBT
- Database migrations do not require PBT
- Deployment scripts do not require PBT
