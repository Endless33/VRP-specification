# Session ≠ Transport

## Status

Public Architecture Specification

Version: 2.0

---

# Abstract

The fundamental design principle of VRP is that a logical session must not be permanently bound to a single transport path.

Traditional communication stacks often associate session continuity with one transport, one interface, or one network attachment.

VRP deliberately separates these concepts.

The runtime treats transport as replaceable while preserving the logical identity of the session.

---

# Motivation

Modern systems continuously encounter changing network conditions.

Examples include:

- Wi-Fi to LTE transitions
- LTE to 5G migration
- CGNAT rebinding
- roaming
- temporary radio loss
- multi-access environments
- edge mobility
- transport degradation
- infrastructure failover

These events should not require rebuilding the logical session if continuity can be preserved.

---

# Architectural Separation

VRP distinguishes two independent layers.

## Logical Session

The logical session represents application continuity.

It identifies an ongoing communication context independently from the transport currently carrying packets.

A logical session has its own lifecycle, authority state, validation history, and continuity guarantees.

---

## Transport

Transport represents only the current delivery mechanism.

Examples include:

- Wi-Fi
- Ethernet
- LTE
- 5G
- satellite
- relay paths
- future transport technologies

The transport may change many times during the lifetime of one logical session.

---

# Design Principle

Transport changes are expected.

Logical session replacement is exceptional.

Whenever possible, runtime adaptation occurs by replacing the transport while preserving the existing logical session.

---

# Runtime Responsibilities

The runtime continuously evaluates transport conditions.

Observable factors may include:

- availability
- latency
- packet loss
- stability
- recovery
- path health

These observations influence runtime decisions.

They do not redefine logical identity.

---

# Continuity

Continuity means that useful work continues despite transport evolution.

Continuity does not imply that every packet is delivered.

Continuity also does not imply zero interruption.

Instead, VRP attempts to preserve logical execution whenever recovery remains possible within runtime policy.

---

# Transport Independence

Applications interacting with VRP should not require awareness of transport replacement events.

Runtime adaptation occurs below the application boundary whenever policy permits.

This reduces coupling between application logic and network topology.

---

# Failure Model

Transport failure is treated as an expected operational event.

Possible outcomes include:

- transport replacement
- recovery
- temporary degradation
- controlled termination

The chosen action depends on runtime policy and current observable state.

---

# Security Considerations

Separating session identity from transport does not remove security requirements.

Runtime validation continues to enforce:

- authority validation
- replay protection
- deterministic transitions
- recovery rules
- protected runtime policies

Transport replacement never bypasses runtime verification.

---

# Public Boundary

This document intentionally describes architectural behavior.

It does not disclose:

- protected algorithms
- internal runtime implementation
- decision heuristics
- protocol encoding
- packet structures
- cryptographic mechanisms

These remain part of the protected VRP runtime.

---

# Related Documents

- RUNTIME_MODEL.md
- AUTHORITY_MODEL.md
- FAILURE_MODEL.md
- SESSION_LIFECYCLE.md
- RFC-0001-Session.md

---

> A transport may change many times.
>
> A logical session should change only when continuity can no longer be safely preserved.