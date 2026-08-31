# Why Not TCP?

## Purpose

This document explains why VRP should not be viewed as a replacement for TCP and why the two technologies solve different engineering problems.

It is intended for architectural evaluation.

It does not disclose implementation details of the protected VRP runtime.

---

# Different Design Goals

TCP is a transport protocol.

VRP is a continuity-oriented runtime architecture.

Although both participate in communication, they solve fundamentally different engineering problems.

---

# TCP Focus

TCP primarily addresses:

- reliable packet delivery
- ordered byte streams
- retransmission
- congestion control
- flow control
- connection management

Its objective is reliable transport between endpoints.

---

# VRP Focus

VRP investigates a different engineering problem.

Core principle:

Session ≠ Transport

The objective is preserving logical runtime session continuity while transport conditions change.

Transport becomes replaceable infrastructure rather than the identity of the session.

---

# Different Questions

TCP asks:

"How should data be delivered reliably?"

VRP asks:

"Should the logical session continue even if the transport changes?"

These questions belong to different architectural layers.

---

# Runtime Continuity

VRP investigates runtime behavior during:

- Wi-Fi ↔ Cellular transitions
- IP migration
- NAT and CGNAT rebinding
- roaming
- temporary communication loss
- authority transitions
- replay attempts
- stale authority injection
- deterministic recovery

The objective is preserving logical runtime continuity rather than maintaining a transport connection.

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

These properties exist independently of any transport implementation.

---

# Complementary Technologies

VRP and TCP should not necessarily be viewed as competing technologies.

TCP may provide reliable transport beneath a continuity-oriented runtime.

The public specification intentionally avoids requiring any particular transport protocol.

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

- that TCP is insufficient
- that TCP should be replaced
- that VRP replaces transport protocols
- that VRP improves TCP performance

VRP investigates a different architectural problem.

---

# Summary

TCP focuses on reliable transport between endpoints.

VRP explores continuity of logical runtime state across changing transport conditions while maintaining deterministic runtime behavior and independently verifiable engineering evidence.

The two approaches solve different engineering problems and may complement one another within the same distributed system architecture.