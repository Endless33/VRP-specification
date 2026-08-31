# Why Not SD-WAN?

## Purpose

This document explains why VRP should not be viewed as a replacement for Software-Defined Wide Area Networking (SD-WAN) and why the two technologies address different engineering problems.

It is intended for architectural evaluation.

It does not disclose implementation details of the protected VRP runtime.

---

# Different Design Goals

SD-WAN primarily manages network infrastructure.

VRP investigates continuity of logical runtime state.

Although both operate across multiple networks, they solve different problems at different architectural layers.

---

# SD-WAN Focus

SD-WAN primarily addresses:

- centralized network management
- path selection
- WAN optimization
- traffic steering
- policy enforcement
- branch connectivity
- network visibility
- operational administration

Its objective is efficient network infrastructure management.

---

# VRP Focus

VRP investigates a different engineering problem.

Core principle:

Session ≠ Transport

The objective is preserving logical session continuity while transport conditions change.

Transport becomes one component of runtime execution rather than the identity of the session.

---

# Different Questions

SD-WAN asks:

"Which network path should traffic use?"

VRP asks:

"Should the logical session continue even if the transport path changes?"

These questions address different architectural concerns.

---

# Runtime Continuity

VRP investigates runtime behavior during:

- WAN failover
- ISP replacement
- Wi-Fi ↔ Cellular transitions
- IP migration
- NAT and CGNAT rebinding
- temporary communication loss
- replay attempts
- stale authority injection
- duplicate execution attempts

The objective is preserving deterministic runtime behavior rather than managing network infrastructure.

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

These properties exist independently of SD-WAN capabilities.

---

# Complementary Technologies

VRP and SD-WAN should not necessarily be viewed as competing technologies.

An SD-WAN deployment may provide network infrastructure while a continuity-oriented runtime independently manages logical session behavior.

The public specification intentionally avoids requiring any particular network architecture.

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

- that SD-WAN is insufficient
- that SD-WAN should be replaced
- that VRP replaces WAN infrastructure
- that VRP performs network orchestration

VRP investigates a different architectural problem.

---

# Summary

SD-WAN focuses on managing network infrastructure and traffic across wide-area networks.

VRP explores continuity of logical runtime state across changing transport conditions while maintaining deterministic runtime decisions and independently verifiable engineering evidence.

The two approaches may be complementary because they operate at different layers of distributed systems.