# Transport Abstraction

## Status

Public Protocol Specification

Version: 2.0

---

# Abstract

Transport Abstraction is one of the fundamental architectural principles of VRP.

The runtime treats transport as an interchangeable delivery mechanism rather than the identity of a communication session.

Applications communicate through a logical session.

The runtime determines which transport currently carries that session.

---

# Purpose

The objective of transport abstraction is to decouple application continuity from network topology.

Changes occurring below the transport boundary should not unnecessarily affect application execution.

The runtime is responsible for adapting to changing network conditions while preserving logical correctness.

---

# Design Goals

Transport abstraction is designed to provide:

- transport independence
- continuity across network changes
- deterministic runtime decisions
- implementation flexibility
- observable runtime behavior
- future protocol compatibility

---

# Transport Independence

VRP intentionally avoids coupling logical sessions to any single transport technology.

The architecture does not assume:

- a fixed IP address
- a permanent network interface
- one access technology
- one communication path
- one physical network

Transport may evolve throughout the lifetime of a logical session.

---

# Supported Transport Classes

The public architecture is transport-neutral.

Examples include:

- Ethernet
- Wi-Fi
- LTE
- 5G
- satellite
- relay networks
- private infrastructure
- future transport technologies

Support for a transport depends on the protected runtime implementation.

---

# Runtime Responsibilities

The runtime continuously evaluates available transports.

Observable responsibilities include:

- monitoring transport health
- evaluating continuity
- validating authority
- coordinating migration
- preserving deterministic behavior
- generating engineering evidence

Applications remain isolated from transport management.

---

# Transport Lifecycle

A transport may progress through observable operational stages.

Examples include:

- discovered
- available
- active
- degraded
- failed
- quarantined
- recovered
- retired

Internal lifecycle implementation remains protected.

---

# Transport Migration

Transport replacement is considered normal runtime behavior.

Migration may occur because of:

- mobility
- degradation
- infrastructure changes
- operator policy
- recovery
- optimization

Migration does not imply creation of a new logical session.

---

# Multiple Transport Availability

Multiple transports may be available simultaneously.

The runtime evaluates available candidates according to protected policy.

Only observable runtime behavior is part of this specification.

Internal ranking algorithms are intentionally excluded.

---

# Failure Isolation

Transport failures remain isolated below the logical session whenever possible.

Observable transport failure does not necessarily imply:

- application failure
- session termination
- authority replacement

The runtime determines whether continuity can safely be preserved.

---

# Transport Validation

Observable transport changes may require runtime validation.

Examples include:

- availability verification
- continuity verification
- authority validation
- runtime consistency
- policy evaluation

Validation protects runtime correctness during transport evolution.

---

# Security Considerations

Changing transport never bypasses runtime verification.

Observable security objectives include:

- replay protection
- authority consistency
- deterministic transitions
- runtime integrity
- policy enforcement

Transport replacement alone does not elevate trust.

---

# Protected Boundary

This document intentionally excludes:

- transport selection algorithms
- routing implementation
- packet scheduling
- internal optimization
- transport scoring
- migration heuristics
- protocol encoding

These remain part of the protected VRP runtime.

---

# Related Documents

- SESSION_NOT_TRANSPORT.md
- MULTIPATH_SELECTION.md
- RUNTIME_MODEL.md
- FAILURE_MODEL.md
- RFC-0003-Transport-Abstraction.md

---

# Summary

Transport is an implementation concern.

Continuity is a runtime concern.

Applications interact with logical sessions rather than individual network technologies.

This separation enables deterministic runtime behavior across changing network environments.

---

> Transport delivers packets.

> The runtime preserves continuity.

> Applications continue their work.