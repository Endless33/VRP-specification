# API Reference

## Status

Public Integration Guide

Version: 2.0

---

# Purpose

This document describes the observable integration interface exposed by the VRP Runtime.

The purpose of this specification is to define how applications conceptually interact with the runtime without disclosing proprietary implementation details.

This document is intentionally implementation-neutral.

---

# Design Philosophy

Applications interact with logical runtime concepts.

Applications do not interact with transport internals.

Applications do not manipulate authority directly.

Applications observe runtime behavior.

The runtime performs deterministic decision making.

---

# Integration Model

```
Application
      │
      ▼
VRP Runtime API
      │
      ▼
Runtime Engine
      │
      ▼
Transport Layer
```

The API represents the public boundary between application logic and the runtime.

---

# Public Runtime Objects

The runtime exposes conceptual objects.

Examples include:

- Runtime
- Session
- Authority
- Transport
- Runtime Event
- Evidence
- Configuration

The internal representation of these objects is intentionally unspecified.

---

# Runtime Operations

Observable runtime operations include:

- initialize runtime
- start runtime
- stop runtime
- create session
- close session
- observe runtime state
- retrieve evidence
- receive events

Execution semantics remain implementation-specific.

---

# Session Operations

Applications may conceptually perform operations such as:

- create
- activate
- observe
- terminate

Applications never directly manipulate authority state.

Authority evolution is controlled by the runtime.

---

# Transport Operations

Applications are not expected to manage transports.

Observable runtime behavior may include:

- transport available
- transport degraded
- transport replaced
- transport recovered

The runtime determines when transport evolution occurs.

---

# Authority Operations

Authority is observable.

Authority is not application-controlled.

Applications may observe:

- authority established
- authority evolved
- authority rejected

Internal authority coordination remains protected.

---

# Runtime Events

Applications may receive observable events.

Examples include:

- RuntimeStarted
- RuntimeStopped
- SessionCreated
- SessionClosed
- TransportChanged
- RecoveryStarted
- RecoveryCompleted
- AuthorityChanged
- ReplayRejected
- DuplicateRejected
- EvidenceAvailable

The event delivery mechanism is intentionally unspecified.

---

# Evidence Access

Applications may obtain observable evidence produced during execution.

Examples include:

- validation summaries
- runtime verdicts
- engineering reports
- recovery summaries
- replay reports

Evidence formats are described elsewhere in this specification.

---

# Error Handling

Observable runtime errors may include:

- initialization failure
- validation failure
- authority rejection
- recovery unavailable
- policy violation

Protected implementation details are intentionally omitted.

---

# Thread Safety

The runtime is designed for deterministic execution in concurrent environments.

Applications should treat runtime operations as coordinated through the runtime itself.

Internal synchronization mechanisms remain protected.

---

# Security Model

Applications do not receive:

- cryptographic secrets
- authority algorithms
- transport scoring
- scheduling logic
- protected runtime state

Only observable behavior is part of the public API model.

---

# Version Compatibility

Public integrations should assume:

- forward-compatible observable behavior whenever practical
- documented behavioral changes between major versions
- stable architectural concepts
- evolving implementation beneath the protected boundary

---

# Non-Goals

This specification does not define:

- programming language bindings
- SDK implementation
- REST endpoints
- gRPC interfaces
- binary protocol
- packet formats

These may vary between deployments.

---

# Protected Boundary

This document intentionally excludes:

- source code
- internal APIs
- runtime implementation
- synchronization algorithms
- protocol encoding
- proprietary optimization

These remain protected components of the VRP Runtime.

---

# Related Documents

- GETTING_STARTED.md
- EMBEDDED_RUNTIME.md
- CALLBACKS.md
- EVENT_MODEL.md
- DEPLOYMENT.md

---

# Summary

The VRP Runtime API exposes observable runtime capabilities rather than implementation details.

Applications interact with stable architectural concepts while the runtime preserves deterministic execution internally.

---

> Applications request.

> The runtime evaluates.

> Observable behavior is returned.