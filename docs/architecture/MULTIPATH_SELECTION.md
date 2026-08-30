# Multipath Selection

## Status

Public Architecture Specification

Version: 2.0

---

# Abstract

VRP treats transport diversity as a normal operating condition rather than an exception.

Multiple transport paths may be available during the lifetime of a logical session.

The runtime continuously evaluates these paths and selects the transport that best satisfies continuity objectives according to protected runtime policy.

---

# Purpose

The purpose of multipath selection is to improve continuity while maintaining deterministic runtime behavior.

Transport replacement is considered an operational activity rather than an application event.

Applications interact with one logical session regardless of how many transport transitions occur underneath.

---

# Transport Diversity

A runtime may observe one or more transport paths simultaneously.

Examples include:

- Ethernet
- Wi-Fi
- LTE
- 5G
- satellite
- relay networks
- future transport technologies

The architecture makes no assumptions about a specific transport implementation.

---

# Continuous Evaluation

The runtime continuously evaluates observable transport conditions.

Examples include:

- availability
- latency
- packet loss
- stability
- responsiveness
- recovery progress

Evaluation is continuous throughout the lifetime of a logical session.

---

# Candidate Paths

Every available transport may become a runtime candidate.

Candidate paths are evaluated according to runtime policy.

Selection does not imply permanent ownership.

The active transport may change as observable conditions evolve.

---

# Runtime Decisions

Observable runtime outcomes may include:

- maintain current transport
- migrate to another transport
- temporarily reject a transport
- quarantine a transport
- recover a transport
- terminate transport usage

The protected runtime determines how these decisions are reached.

---

# Explicit Operational States

The runtime may distinguish between operational states representing transport availability.

Examples include:

- healthy
- degraded
- failed
- quarantined
- recovered

The precise implementation of these states is protected.

Public documentation describes only externally observable behavior.

---

# Recovery

Recovery is an explicit runtime activity.

Recovery does not assume that a previously unavailable transport immediately becomes suitable for production traffic.

The runtime validates recovery before allowing continued participation.

---

# Deterministic Selection

Equivalent observable conditions should produce equivalent observable transport decisions.

This property improves:

- reproducibility
- testing
- validation
- operational predictability

Deterministic selection remains a fundamental design objective.

---

# Failure Handling

Transport failure is considered an expected event.

Possible observable outcomes include:

- migration
- recovery
- continued degradation
- transport retirement

Failure handling aims to preserve logical continuity whenever runtime policy allows.

---

# Security Considerations

Transport selection never bypasses runtime validation.

Changing transport does not automatically imply:

- authority change
- session recreation
- trust elevation
- policy relaxation

Runtime verification remains active during transport evolution.

---

# Protected Boundary

This document intentionally excludes:

- transport scoring algorithms
- ranking implementation
- scheduling heuristics
- runtime optimization strategies
- internal scoring weights
- proprietary selection logic

These mechanisms remain part of the protected VRP runtime.

---

# Related Documents

- SESSION_NOT_TRANSPORT.md
- RUNTIME_MODEL.md
- AUTHORITY_MODEL.md
- FAILURE_MODEL.md
- RFC-0008-Multipath.md

---

> A transport may change.

> A session should continue.

> Runtime policy determines when change is appropriate.