# Runtime Boundaries

## Status

Public Security Documentation

Version: 1.0

---

# Purpose

This document defines the architectural boundaries of the Protected VRP Runtime.

Its purpose is to describe which responsibilities belong to the runtime and which remain outside its scope.

The document defines observable responsibilities rather than implementation details.

---

# Architectural Principle

The runtime owns execution.

Everything outside the runtime is considered an external dependency.

Execution correctness must never depend upon external assumptions.

---

# Runtime Boundary

The Protected Runtime begins when an application invokes the Runtime API.

It ends where transport infrastructure begins.

```
Application
      │
      ▼
Runtime API
      │
      ▼
=============================
 Protected VRP Runtime
=============================
      │
      ▼
Transport Infrastructure
      │
      ▼
External Network
```

Everything inside the runtime boundary is implementation-defined.

Everything outside the boundary is treated as external.

---

# Responsibilities Inside the Runtime

The runtime is responsible for:

- Logical Session lifecycle
- Authority Epoch progression
- Runtime State Machine
- Replay Protection
- Recovery decisions
- Multipath Selection
- Runtime event generation
- Evidence generation
- Runtime invariants
- Deterministic execution

These responsibilities are never delegated outside the runtime.

---

# Responsibilities Outside the Runtime

The runtime does not control:

- application business logic
- operating system scheduling
- hardware failures
- cloud infrastructure
- network providers
- transport availability
- external routing
- administrator decisions

The runtime observes these conditions.

It does not own them.

---

# Runtime Inputs

Observable runtime inputs may include:

- application requests
- transport events
- network conditions
- authority events
- recovery triggers
- configuration
- runtime policy

Input processing remains implementation-specific.

---

# Runtime Outputs

Observable runtime outputs may include:

- runtime events
- authority transitions
- transport transitions
- recovery notifications
- engineering evidence
- validation verdicts
- runtime status

Outputs form the observable engineering surface.

---

# Runtime Decisions

Only the runtime determines:

- authority acceptance
- authority rejection
- replay rejection
- transport eligibility
- recovery execution
- session termination

External systems may request.

The runtime decides.

---

# Boundary Invariants

The runtime MUST preserve:

- one Logical Session
- one canonical authority
- deterministic execution
- replay resistance
- recovery correctness
- observable consistency
- engineering evidence integrity

These invariants define runtime correctness.

---

# Failure Handling

External failures do not automatically become runtime failures.

Examples include:

- transport interruption
- infrastructure restart
- temporary network loss
- degraded connectivity

The runtime evaluates whether continuity remains architecturally correct.

---

# Protected Implementation

The following remain permanently inside the protected boundary:

- source code
- internal algorithms
- scheduler implementation
- transport scoring
- optimization logic
- synchronization mechanisms
- runtime heuristics
- packet encoding
- internal data structures

These are not required for engineering validation.

---

# Engineering Validation

Independent reviewers should validate:

- observable runtime behavior
- deterministic decisions
- authority consistency
- replay rejection
- recovery correctness
- engineering evidence

Implementation inspection is unnecessary.

---

# Relationship to Other Documents

This document complements:

- THREAT_MODEL.md
- ATTACK_TREE.md
- TRUST_BOUNDARY.md
- SECURITY_MODEL.md
- OPERATOR_TRUST_MODEL.md

It also supports:

- RFC-0009 — Security Boundary
- ADR-0003 — Why the Runtime is Protected

---

# Summary

The Protected Runtime owns execution correctness.

Infrastructure may change.

Transport may change.

Applications may change.

The runtime remains responsible for preserving architectural invariants.

---

## Design Principles

- Own execution.
- Reject invalid state.
- Preserve deterministic behavior.
- Protect implementation.
- Produce observable evidence.
- Preserve architectural invariants.