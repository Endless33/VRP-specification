# Why Not WireGuard?

## Purpose

This document explains why VRP should not be viewed as a replacement for WireGuard and why the two technologies address different engineering problems.

It is intended for architectural evaluation.

It does not disclose implementation details of the protected VRP runtime.

---

# Different Design Goals

WireGuard is a secure VPN protocol.

VRP investigates continuity of logical runtime state across changing transport conditions.

Although both involve network communication, they solve different engineering problems.

---

# WireGuard Focus

WireGuard primarily addresses:

- secure encrypted tunnels
- low implementation complexity
- modern cryptography
- efficient packet processing
- VPN connectivity
- secure peer communication

Its objective is providing secure network transport.

---

# VRP Focus

VRP investigates a different engineering problem.

Core principle:

Session ≠ Transport

The objective is preserving logical session continuity while transport conditions change.

Transport is treated as replaceable infrastructure rather than the identity of the session.

---

# Different Questions

WireGuard asks:

"How can two peers communicate securely?"

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
- duplicate execution attempts
- deterministic recovery

The objective is preserving runtime continuity rather than establishing encrypted tunnels.

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

These properties exist independently of any specific VPN protocol.

---

# Complementary Technologies

VRP and WireGuard should not necessarily be viewed as competing technologies.

Depending on implementation goals, WireGuard may provide secure transport while a continuity-oriented runtime independently manages logical session behavior.

The public specification intentionally avoids requiring any particular VPN protocol.

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

- that WireGuard is insufficient
- that WireGuard should be replaced
- that VRP replaces VPN protocols
- that VRP improves WireGuard performance

VRP investigates a different architectural problem.

---

# Summary

WireGuard focuses on secure encrypted communication between network peers.

VRP explores continuity of logical runtime state across changing transport conditions while maintaining deterministic runtime decisions and independently verifiable engineering evidence.

The two approaches may be complementary because they solve different engineering problems at different architectural layers.