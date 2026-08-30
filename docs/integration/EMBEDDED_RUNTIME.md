# Embedded Runtime

## Status

Public Integration Guide

Version: 2.0

---

# Abstract

The VRP Runtime is designed to operate as an embedded software component within an existing application rather than as a standalone operating environment.

Applications continue to own their business logic.

The runtime provides continuity management, authority validation and deterministic execution beneath the application layer.

This document describes the observable integration model.

Protected implementation details remain outside the scope of this specification.

---

# Design Philosophy

Applications should not need to understand transport management.

Applications should not manage authority evolution.

Applications should not implement replay protection.

Applications should focus on business logic.

The embedded runtime manages continuity.

---

# Architectural Position

The runtime sits between the application and the underlying transport infrastructure.

```
Application
        │
        ▼
Business Logic
        │
        ▼
VRP Embedded Runtime
        │
        ▼
Transport Layer
        │
        ▼
Operating System
        │
        ▼
Network
```

The application interacts only with the runtime.

---

# Runtime Responsibilities

The embedded runtime is responsible for observable functions including:

- session management
- authority validation
- transport evaluation
- recovery coordination
- replay rejection
- duplicate execution prevention
- evidence generation
- deterministic runtime decisions

These responsibilities remain isolated from application logic.

---

# Application Responsibilities

Applications remain responsible for:

- business rules
- user workflows
- persistence
- authorization
- domain logic
- application state

The runtime intentionally does not replace application architecture.

---

# Runtime Independence

The embedded runtime does not require applications to be rewritten around a proprietary programming model.

Applications remain free to choose:

- architecture
- deployment strategy
- programming language
- infrastructure
- communication model

The runtime integrates beneath these concerns.

---

# Session Interaction

Applications interact with logical sessions rather than individual transports.

Observable application operations include:

- creating sessions
- closing sessions
- receiving runtime events
- receiving recovery notifications
- observing runtime state

Transport evolution remains transparent whenever continuity is preserved.

---

# Runtime Events

The embedded runtime may expose observable events including:

- session created
- authority updated
- transport changed
- recovery started
- recovery completed
- replay rejected
- duplicate rejected
- runtime terminated

The public specification does not define event implementation.

---

# Failure Handling

Applications are informed about observable runtime outcomes.

Examples include:

- recovery completed
- recovery unavailable
- authority changed
- runtime terminated

Applications are not expected to implement recovery algorithms themselves.

---

# Security Model

The runtime validates execution independently of application logic.

Observable security responsibilities include:

- authority consistency
- replay rejection
- deterministic transitions
- runtime integrity

Security-critical implementation remains protected.

---

# Engineering Integration

Evaluation teams should verify:

- deterministic runtime behavior
- continuity preservation
- authority consistency
- observable recovery
- evidence generation

Integration success is measured through observable behavior.

---

# Protected Boundary

This specification intentionally excludes:

- runtime APIs
- implementation interfaces
- internal scheduling
- protocol encoding
- synchronization algorithms
- runtime optimization
- proprietary architecture

These remain protected.

---

# Related Documents

- GETTING_STARTED.md
- SESSION_LIFECYCLE.md
- EVENT_FLOW.md
- RUNTIME_MODEL.md
- API_REFERENCE.md

---

# Summary

Applications own business logic.

The embedded runtime owns continuity.

This separation enables applications to benefit from deterministic runtime behavior without requiring knowledge of protected implementation.

---

> Applications solve business problems.

> The runtime preserves execution continuity.

> Both evolve independently.