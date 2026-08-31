# Why Not UDP?

## Purpose

This document explains why VRP should not be viewed as a replacement for UDP and why the two technologies solve different engineering problems.

It is intended for architectural evaluation.

It does not disclose implementation details of the protected VRP runtime.

---

# Different Design Goals

UDP is a transport protocol.

VRP is a continuity-oriented runtime architecture.

Although both participate in network communication, they operate at different architectural layers.

---

# UDP Focus

UDP primarily addresses:

- lightweight packet delivery
- minimal protocol overhead
- low latency
- application-controlled reliability
- connectionless communication

Its objective is efficient packet transport.

---

# VRP Focus

VRP investigates a different engineering problem.

Core principle:

Session ≠ Transport

The objective is preserving logical runtime session continuity while transport conditions change.

Transport is treated as replaceable infrastructure rather than the identity of the session.

---

# Different Questions

UDP asks:

"How can packets be delivered with minimal overhead?"

VRP asks:

"Should the logical session continue even if the transport changes?"

These questions belong to different architectural layers.

---

# Runtime Continuity

VRP investigates runtime behavior during:

- Wi-Fi ↔ Cellular transitions
- roaming
- IP migration
- NAT and CGNAT rebinding
- temporary communication loss
- replay attempts
- stale authority injection
- deterministic recovery

The objective is preserving logical runtime continuity rather than providing packet transport.

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

These properties exist independently of the transport protocol.

---

# Complementary Technologies

VRP and UDP should not necessarily be viewed as competing technologies.

UDP may provide packet transport beneath a continuity-oriented runtime.

The public specification intentionally avoids requiring any specific transport protocol.

---

# Evaluation Boundary

This document discusses architectural concepts only.

It does not describe:

- protected runtime implementation
- transport scoring mechanisms
- synchronization algorithms
- proprietary runtime logic
- implementation heuristics

---

# Non-Goals

This document does not claim:

- that UDP is insufficient
- that UDP should be replaced
- that VRP replaces transport protocols
- that VRP improves UDP performance

VRP investigates a different architectural problem.

---

# Summary

UDP focuses on lightweight packet transport.

VRP explores continuity of logical runtime state across changing transport conditions while maintaining deterministic runtime behavior and independently verifiable engineering evidence.

The two approaches solve different engineering problems and may complement one another within the same distributed system architecture.