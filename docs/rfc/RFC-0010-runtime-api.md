# RFC-0010 — Runtime API

**Document Status:** Public Specification

**RFC Number:** RFC-0010

**Version:** 1.0

**Category:** Public Runtime Interface

---

# Abstract

This document defines the conceptual Runtime API exposed by the Veil Routing Protocol (VRP).

The Runtime API represents the observable interface between an application and the protected VRP Runtime.

The API described in this document is architectural rather than language-specific.

Implementation details remain part of the protected runtime.

---

# 1. Introduction

Applications require a stable interaction model.

The Runtime API provides that interaction without exposing implementation.

Applications interact with observable runtime concepts.

The runtime performs deterministic execution internally.

---

# 2. Terminology

The key words **MUST**, **MUST NOT**, **SHOULD**, **SHOULD NOT**, and **MAY** are interpreted as described in RFC 2119.

---

# 3. Design Goals

The Runtime API is designed to provide:

- architectural stability
- implementation independence
- deterministic behavior
- transport independence
- observable runtime events
- reproducible engineering validation

The Runtime API is intentionally minimal.

---

# 4. Architectural Position

```
Application
      │
      ▼
Runtime API
      │
      ▼
Protected Runtime
      │
      ▼
Transport Infrastructure
```

Applications communicate with the Runtime API.

The Runtime API communicates with the protected implementation.

---

# 5. Runtime Object

Every application communicates with one Runtime object.

The Runtime object conceptually represents:

- initialization
- execution
- recovery
- event delivery
- shutdown

Its implementation remains proprietary.

---

# 6. Session Operations

The Runtime API conceptually supports:

- Create Session
- Close Session
- Observe Session
- Query Session State

Applications MUST NOT manipulate internal runtime state directly.

---

# 7. Runtime Lifecycle

The Runtime API conceptually supports:

- Initialize
- Start
- Stop
- Shutdown

Lifecycle ordering SHOULD remain deterministic.

---

# 8. Event Delivery

Applications MAY observe runtime events including:

- session events
- authority events
- transport events
- recovery events
- security events
- evidence events

Delivery mechanisms remain implementation-specific.

---

# 9. Runtime Queries

Applications MAY request observable information including:

- runtime status
- session status
- transport status
- authority status
- evidence availability

Internal runtime state remains protected.

---

# 10. Recovery Interaction

Applications MAY receive recovery notifications.

Applications SHOULD NOT implement recovery logic themselves.

Recovery decisions remain the responsibility of the runtime.

---

# 11. Evidence Access

Applications MAY obtain engineering evidence generated during execution.

Evidence MAY include:

- validation summaries
- runtime verdicts
- engineering reports
- observable event history

Evidence supports independent engineering assessment.

---

# 12. Thread Safety

The Runtime API SHOULD support concurrent application interaction.

Internal synchronization mechanisms remain implementation-specific.

Applications SHOULD treat runtime operations as deterministic.

---

# 13. Error Handling

Observable runtime errors MAY include:

- initialization failure
- validation failure
- policy rejection
- recovery unavailable
- runtime termination

Implementation-specific diagnostics remain outside this specification.

---

# 14. Version Compatibility

Future Runtime API versions SHOULD preserve conceptual compatibility whenever practical.

Observable runtime behavior SHOULD remain stable across compatible releases.

---

# 15. Engineering Validation

Independent reviewers SHOULD validate:

- deterministic behavior
- observable events
- evidence generation
- recovery notifications
- runtime consistency

Implementation disclosure is unnecessary.

---

# 16. Security Considerations

The Runtime API MUST NOT expose:

- source code
- proprietary algorithms
- cryptographic material
- scheduling logic
- implementation heuristics

Observable runtime behavior remains the evaluation boundary.

---

# 17. Non-Goals

This RFC does not define:

- Go interfaces
- C APIs
- Rust traits
- REST endpoints
- gRPC services
- binary interfaces

Language bindings remain implementation-specific.

---

# 18. Related RFCs

RFC-0001 — Logical Session Identity

RFC-0004 — Runtime State Machine

RFC-0005 — Evidence Model

RFC-0008 — Multipath Selection

RFC-0009 — Security Boundary

RFC-0011 — Pilot Integration

---

# 19. Summary

The Runtime API provides a stable conceptual interface between applications and the protected VRP Runtime.

Applications interact with observable runtime behavior.

Protected implementation remains confidential.

---

## Normative Requirements

- Applications **MUST NOT** manipulate internal runtime state.
- Runtime behavior **SHOULD** remain deterministic.
- Runtime events **SHOULD** be observable.
- Evidence **MAY** be exposed through the Runtime API.
- Protected implementation **MUST NOT** be exposed through the Runtime API.

---

## Design Principle

> Applications communicate through concepts.

> The runtime performs deterministic execution.

> Protected implementation remains behind the Runtime API.