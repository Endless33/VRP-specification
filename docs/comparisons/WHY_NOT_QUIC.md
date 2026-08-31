# Why Not QUIC?

## Purpose

This document explains why VRP should not be viewed as a replacement for QUIC and why the two technologies address different engineering problems.

It is intended for architectural evaluation.

It does not disclose implementation details of the protected VRP runtime.

---

# Different Design Goals

QUIC is a transport protocol.

VRP is a continuity-oriented runtime architecture.

Although both deal with communication, they operate at different architectural levels.

---

# QUIC Focus

QUIC primarily addresses:

- transport efficiency
- reduced connection establishment latency
- congestion control
- multiplexed streams
- packet loss recovery
- encrypted transport

Its objective is efficient transport between two endpoints.

---

# VRP Focus

VRP investigates a different engineering problem.

Core principle:

Session ≠ Transport

The objective is preserving logical session continuity while transport conditions change.

Transport becomes one component of the runtime rather than the runtime itself.

---

# Different Questions

QUIC asks:

"How can packets move efficiently?"

VRP asks:

"Should the logical session continue even if transport changes?"

These questions are related but not equivalent.

---

# Transport Independence

VRP does not depend on one specific transport protocol.

A runtime may operate across environments involving:

- TCP
- UDP
- QUIC
- future transports
- proprietary transports

Transport selection is outside the architectural principle.

---

# Runtime Continuity

VRP investigates runtime behavior during events such as:

- IP migration
- Wi-Fi ↔ Cellular transitions
- roaming
- NAT rebinding
- transport interruption
- temporary communication loss

The focus is preserving deterministic runtime behavior rather than optimizing transport performance.

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

These properties exist independently of any particular transport protocol.

---

# Complementary Technologies

VRP and QUIC should not necessarily be viewed as competing technologies.

Depending on implementation goals, QUIC may be one transport used beneath a continuity-oriented runtime.

The public specification intentionally avoids requiring any specific transport protocol.

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

- that QUIC is insufficient
- that QUIC should be replaced
- that VRP replaces transport protocols
- that VRP improves QUIC performance

VRP investigates a different architectural problem.

---

# Summary

QUIC focuses on efficient transport.

VRP explores continuity of logical runtime state across changing transport conditions.

The two technologies address different layers of system behavior and may be complementary rather than mutually exclusive.