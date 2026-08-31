# Why Not OpenVPN?

## Purpose

This document explains why VRP should not be viewed as a replacement for OpenVPN and why the two technologies address different engineering problems.

It is intended for architectural evaluation.

It does not disclose implementation details of the protected VRP runtime.

---

# Different Design Goals

OpenVPN is a VPN protocol designed to establish secure communication across untrusted networks.

VRP investigates continuity of logical runtime state while network transports evolve during normal operation.

Although both involve secure communication, they solve different engineering problems.

---

# OpenVPN Focus

OpenVPN primarily addresses:

- encrypted VPN tunnels
- authenticated communication
- secure remote access
- site-to-site connectivity
- client-server VPN deployment
- transport confidentiality

Its objective is secure network communication.

---

# VRP Focus

VRP investigates a different engineering problem.

Core principle:

Session ≠ Transport

The objective is preserving logical session continuity while transport conditions change.

Transport is treated as replaceable infrastructure rather than the identity of the session.

---

# Different Questions

OpenVPN asks:

"How can communication remain secure across an untrusted network?"

VRP asks:

"Should the logical session continue even if the underlying transport changes?"

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

The objective is preserving runtime continuity rather than maintaining a VPN tunnel.

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

These properties exist independently of any particular VPN implementation.

---

# Complementary Technologies

VRP and OpenVPN should not necessarily be viewed as competing technologies.

Depending on deployment requirements, OpenVPN may provide encrypted transport while a continuity-oriented runtime independently manages logical session behavior.

The public specification intentionally avoids requiring any specific VPN technology.

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

- that OpenVPN is insufficient
- that OpenVPN should be replaced
- that VRP replaces VPN protocols
- that VRP improves OpenVPN performance

VRP investigates a different architectural problem.

---

# Summary

OpenVPN focuses on secure encrypted communication between distributed systems.

VRP explores continuity of logical runtime state across changing transport conditions while maintaining deterministic runtime decisions and independently verifiable engineering evidence.

The two approaches may complement one another because they address different layers of distributed system architecture.