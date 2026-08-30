# Operator Trust Model

## Status

Public Security Documentation

Version: 1.0

---

# Purpose

This document defines the trust relationship between system operators and the protected VRP Runtime.

Its objective is to clearly distinguish operational authority from runtime authority.

System administration does not imply control over runtime correctness.

---

# Security Philosophy

Operators manage infrastructure.

The runtime manages execution.

These responsibilities are intentionally separated.

Operational privileges must never redefine architectural correctness.

---

# Scope

This document applies to:

- system administrators
- infrastructure operators
- cloud operators
- DevOps teams
- platform engineers
- site reliability engineers
- evaluation participants

---

# Operator Responsibilities

Operators are responsible for:

- runtime deployment
- infrastructure provisioning
- operating system maintenance
- network configuration
- service availability
- monitoring
- log collection
- backup procedures

These responsibilities do not include runtime decision making.

---

# Runtime Responsibilities

The Protected Runtime is responsible for:

- Logical Session management
- Authority Epoch progression
- Replay Protection
- Runtime State Machine
- Recovery decisions
- Multipath Selection
- Evidence generation

These responsibilities remain inside the protected runtime.

---

# Trust Separation

```
Operator
      │
      │ Infrastructure
      ▼
Deployment Environment
      │
      ▼
Protected Runtime
      │
      ▼
Runtime Decisions
```

Operational control ends at the runtime boundary.

Runtime correctness begins inside the runtime.

---

# Administrative Authority

Operators MAY:

- install the runtime
- start the runtime
- stop the runtime
- update configuration
- collect observable evidence
- monitor execution

Operators MUST NOT assume authority over:

- runtime decisions
- authority progression
- replay validation
- transport selection
- deterministic execution

---

# Configuration

Configuration influences deployment.

Configuration does not redefine architectural invariants.

Observable runtime correctness must remain independent from operational preference.

---

# Monitoring

Operators SHOULD observe:

- runtime health
- resource utilization
- event generation
- evidence availability
- validation status

Monitoring does not alter runtime behavior.

---

# Failure Handling

Operators MAY respond to:

- infrastructure failure
- hardware replacement
- operating system maintenance
- network outage

The runtime determines whether continuity remains possible.

Recovery decisions remain internal.

---

# Security Assumptions

Operators are considered trusted to operate infrastructure.

The runtime does not require operators to understand protected implementation.

Observable behavior remains sufficient for engineering evaluation.

---

# Least Privilege

Operational permissions SHOULD follow the principle of least privilege.

Administrative capability SHOULD be limited to operational responsibilities.

Implementation confidentiality remains preserved.

---

# Engineering Validation

Evaluation teams SHOULD verify that:

- operator actions do not violate runtime invariants
- infrastructure changes preserve observable correctness
- runtime decisions remain deterministic
- engineering evidence remains reproducible

Observable behavior determines engineering conclusions.

---

# Out of Scope

This document does not define:

- operating system security
- identity management
- cloud IAM
- infrastructure compliance
- organizational policy

These remain deployment-specific concerns.

---

# Related Documents

TRUST_BOUNDARY.md

SECURITY_MODEL.md

RUNTIME_BOUNDARIES.md

RFC-0009 — Security Boundary

ADR-0003 — Why the Runtime is Protected

---

# Summary

Operators administer infrastructure.

The runtime administers execution.

This separation ensures that operational authority never becomes architectural authority.

Protected implementation remains protected.

---

## Design Principle

Infrastructure belongs to operators.

Execution belongs to the runtime.

Architectural correctness belongs to neither.