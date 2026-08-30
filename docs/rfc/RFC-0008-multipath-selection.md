# RFC-0008 — Multipath Selection

**Document Status:** Public Specification

**RFC Number:** RFC-0008

**Version:** 1.0

**Category:** Runtime Architecture

---

# Abstract

This document defines the observable Multipath Selection model of the Veil Routing Protocol (VRP).

Multipath Selection enables the runtime to evaluate multiple transport opportunities while preserving Logical Session continuity, deterministic runtime behavior and canonical authority.

This specification defines observable architectural behavior only.

Selection algorithms, scoring heuristics and optimization mechanisms remain part of the protected VRP Runtime.

---

# 1. Introduction

Modern communication environments rarely consist of a single transport.

Runtime execution may observe multiple simultaneous transport opportunities including:

- Ethernet
- Wi-Fi
- LTE
- 5G
- relay infrastructure
- satellite connectivity
- future transport technologies

The runtime evaluates available transports while preserving architectural invariants.

---

# 2. Terminology

The key words **MUST**, **MUST NOT**, **SHOULD**, **SHOULD NOT**, and **MAY** are interpreted as described in RFC 2119.

---

# 3. Objective

The objective of Multipath Selection is to maintain the best observable execution path without compromising:

- Logical Session identity
- canonical authority
- deterministic execution
- replay protection
- engineering reproducibility

Transport optimization is secondary to runtime correctness.

---

# 4. Architectural Model

```
Logical Session
        │
        ▼
Authority Layer
        │
        ▼
Multipath Evaluation
        │
        ▼
Candidate Transports
        │
        ▼
Selected Transport
```

Applications remain unaware of transport selection decisions.

---

# 5. Candidate Paths

The runtime MAY evaluate multiple candidate transports simultaneously.

Candidate transports MAY differ in:

- latency
- stability
- availability
- reliability
- operational characteristics

The public specification intentionally does not define evaluation metrics.

---

# 6. Selection

The runtime selects one observable active transport for execution.

Selection MUST preserve architectural invariants.

Selection algorithms remain proprietary.

---

# 7. Transport Evolution

The selected transport MAY change during execution.

Examples include:

Wi-Fi

↓

LTE

↓

5G

↓

Relay

↓

Ethernet

Logical Session identity MUST remain unchanged.

---

# 8. Explicit Runtime States

A transport MAY enter observable operational states including:

- Healthy
- Degraded
- Failed
- Quarantined
- Recovered

Observable runtime behavior SHOULD preserve explicit operational state until an explicit recovery decision occurs.

Metric observations alone MUST NOT silently invalidate explicit operational state.

---

# 9. Recovery

Recovery MAY restore transport eligibility.

Recovery MUST be explicitly authorized by runtime policy.

Recovery MUST preserve:

- deterministic execution
- canonical authority
- replay protection

Recovery mechanisms remain implementation-specific.

---

# 10. Failure Handling

Transport failure MUST NOT automatically terminate the Logical Session.

The runtime evaluates whether:

- continuity may continue
- recovery should begin
- termination is required

Correctness takes precedence over availability.

---

# 11. Determinism

Equivalent observable runtime conditions SHOULD produce equivalent transport decisions.

Deterministic behavior improves:

- engineering validation
- operational confidence
- reproducibility

Implementation mechanisms remain protected.

---

# 12. Engineering Validation

Independent engineering validation MAY verify:

- deterministic transport selection
- explicit state preservation
- transport recovery
- continuity preservation
- engineering reproducibility

Validation is based upon observable behavior.

---

# 13. Engineering Evidence

Observable evidence MAY include:

- transport timeline
- selection history
- recovery events
- engineering verdicts
- validation summaries

Evidence supports independent technical assessment.

---

# 14. Engineering Invariants

Multipath Selection MUST preserve:

- Logical Session identity
- canonical authority
- deterministic execution
- replay protection
- explicit operational state
- observable consistency

Selection MUST NOT violate these invariants.

---

# 15. Security Considerations

Multipath Selection improves runtime resilience while maintaining deterministic behavior.

Selection MUST NOT permit:

- stale authority resurrection
- replay acceptance
- invalid state transition
- unauthorized transport activation

Protected implementation remains confidential.

---

# 16. Non-Goals

This RFC does not define:

- scoring algorithms
- ranking heuristics
- transport weighting
- optimization strategy
- scheduling algorithms
- concurrency implementation
- runtime internals

These remain proprietary implementation details.

---

# 17. Related RFCs

RFC-0001 — Logical Session Identity

RFC-0002 — Authority Epochs

RFC-0003 — Transport Abstraction

RFC-0006 — Replay Protection

RFC-0007 — Failure Recovery

RFC-0009 — Security Boundary

---

# 18. Summary

Multipath Selection enables the runtime to evaluate transport opportunities while preserving deterministic runtime behavior.

Observable transport evolution remains reproducible.

Protected selection logic remains protected.

Engineering validation focuses on runtime behavior rather than implementation.

---

## Normative Requirements

- The runtime **MUST** maintain one observable active transport.
- Transport evolution **MUST NOT** redefine the Logical Session.
- Explicit FAILED and QUARANTINED states **MUST NOT** be cleared solely by metric updates.
- Recovery **MUST** occur only through explicit runtime authorization.
- Multipath Selection **SHOULD** remain deterministic.

---

## Design Principle

> Multiple transports may exist.

> One transport executes.

> The Logical Session remains unchanged.