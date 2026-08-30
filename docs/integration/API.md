# Runtime API

## Status

Public Integration Documentation

Version: 1.0

---

# Purpose

This document defines the public architectural Runtime API of the Veil Routing Protocol (VRP).

The Runtime API provides the integration boundary between an application and the Protected VRP Runtime.

The API intentionally exposes architectural concepts rather than implementation details.

Language-specific interfaces are outside the scope of this specification.

---

# Engineering Philosophy

Applications should communicate with a stable runtime abstraction.

The runtime should hide implementation complexity.

Observable behavior should remain stable even when internal implementation evolves.

The API is an architectural contract.

---

# Design Goals

The Runtime API is designed to provide:

- stable integration
- deterministic behavior
- transport independence
- observable runtime events
- minimal application coupling
- implementation independence

The API intentionally remains small.

---

# Architectural Position

```
Application
      │
      ▼
Runtime API
      │
      ▼
Protected VRP Runtime
      │
      ▼
Transport Infrastructure
```

Applications never communicate directly with transport infrastructure.

---

# Runtime Instance

An application interacts with one Runtime Instance.

A Runtime Instance represents one observable execution environment.

The runtime internally manages:

- sessions
- authority
- recovery
- transport evolution
- evidence generation

Implementation remains protected.

---

# Session Operations

The Runtime API conceptually supports:

- Create Session
- Observe Session
- Close Session
- Query Session Status

Applications interact with Logical Sessions.

Applications never manipulate runtime internals.

---

# Runtime Lifecycle

The Runtime API supports the observable lifecycle:

```
Create Runtime

↓

Initialize

↓

Ready

↓

Running

↓

Stopping

↓

Stopped
```

Lifecycle transitions remain deterministic.

---

# Runtime Events

Applications may subscribe to observable runtime events.

Examples include:

- SessionCreated
- SessionActivated
- AuthorityChanged
- RecoveryStarted
- RecoveryCompleted
- ReplayRejected
- TransportChanged
- RuntimeStopped

Event delivery mechanisms remain implementation-specific.

---

# Runtime Queries

Applications may query observable runtime state.

Typical information includes:

- runtime status
- session status
- authority status
- transport status
- recovery status
- evidence availability

Protected implementation state is never exposed.

---

# Runtime Commands

Applications may request operations including:

- initialize runtime
- create session
- terminate session
- request shutdown

The runtime determines whether each request is valid.

Applications issue requests.

The runtime makes decisions.

---

# Error Model

The Runtime API may return observable errors including:

- invalid request
- session unavailable
- policy rejection
- recovery unavailable
- runtime stopped

Internal implementation diagnostics remain protected.

---

# Thread Safety

Applications may interact concurrently with the Runtime API.

The runtime is responsible for maintaining deterministic behavior.

Internal synchronization remains implementation-defined.

---

# Version Compatibility

Future Runtime API versions should preserve conceptual compatibility whenever practical.

Applications should depend upon architectural behavior rather than implementation details.

---

# Security Boundary

The Runtime API intentionally does not expose:

- source code
- internal algorithms
- transport scoring
- authority implementation
- scheduler behavior
- synchronization primitives
- runtime heuristics

Observable runtime behavior remains the integration surface.

---

# Engineering Validation

Independent engineering validation should verify:

- deterministic API behavior
- session lifecycle
- authority progression
- replay rejection
- recovery behavior
- observable runtime events

Implementation inspection is unnecessary.

---

# Relationship to Other Documents

This document complements:

- EMBEDDING.md
- CALLBACKS.md
- EVENTS.md
- CONFIGURATION.md
- TRANSPORTS.md

It also supports:

- RFC-0010 — Runtime API
- RFC-0011 — Pilot Integration

---

# Summary

The Runtime API provides a stable architectural boundary between applications and the Protected VRP Runtime.

Applications interact with observable runtime concepts.

The runtime preserves deterministic execution.

Implementation remains protected.

---

## Design Principles

- Expose architecture.
- Hide implementation.
- Preserve deterministic behavior.
- Keep applications transport-independent.
- Maintain a stable integration contract.