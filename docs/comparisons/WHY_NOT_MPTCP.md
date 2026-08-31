# Why Not Multipath TCP (MPTCP)?

## Purpose

This document explains why VRP should not be viewed as a replacement for Multipath TCP (MPTCP) and why the two technologies solve different engineering problems.

It is intended for architectural evaluation.

It does not disclose implementation details of the protected VRP runtime.

---

# Different Design Goals

MPTCP extends TCP by allowing multiple network paths to carry a single transport connection.

VRP investigates continuity of logical runtime state independently of the underlying transport.

Although both involve multiple network paths, they operate at different architectural layers.

---

# MPTCP Focus

Multipath TCP primarily addresses:

- bandwidth aggregation
- path redundancy
- transport resilience
- TCP compatibility
- transport-level failover
- efficient use of multiple interfaces

Its objective is improving transport behavior.

---

# VRP Focus

VRP investigates a different engineering problem.

Core principle:

Session ≠ Transport

The objective is preserving logical session continuity while transport conditions change.

Transport is treated as replaceable infrastructure rather than the identity of the session.

---

# Different Questions

MPTCP asks:

"How can multiple network paths improve one transport connection?"

VRP asks:

"Should the logical session continue even if the transport itself changes?"

These are fundamentally different architectural questions.

---

# Runtime Continuity

VRP investigates runtime behavior during events such as:

- Wi-Fi ↔ Cellular transitions
- IP migration
- NAT and CGNAT rebinding
- roaming
- transport interruption
- temporary communication loss
- authority changes
- replay attempts

The objective is deterministic runtime behavior rather than transport optimization.

---

# Observable Evaluation

Engineering teams may evaluate:

- logical session continuity
- deterministic runtime decisions
- replay rejection
- stale authority rejection
- duplicate execution protection
- recovery behavior
- evidence generation
- independent verification

These properties are independent of any specific transport protocol.

---

# Complementary Technologies

VRP and MPTCP should not necessarily be viewed as competing technologies.

Depending on implementation goals, MPTCP may be one transport technology operating beneath a continuity-oriented runtime.

The public specification intentionally does not require any particular transport protocol.

---

# Evaluation Boundary

This document discusses architectural concepts only.

It does not describe:

- protected runtime implementation
- transport selection logic
- synchronization mechanisms
- proprietary algorithms
- implementation heuristics

---

# Non-Goals

This document does not claim:

- that MPTCP is insufficient
- that MPTCP should be replaced
- that VRP replaces transport protocols
- that VRP improves MPTCP performance

VRP investigates a different architectural problem.

---

# Summary

MPTCP focuses on improving transport behavior across multiple network paths.

VRP explores continuity of logical runtime state across changing transport conditions while maintaining deterministic runtime behavior and independently verifiable engineering evidence.