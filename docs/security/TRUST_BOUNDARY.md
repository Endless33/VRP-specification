# Trust Boundary

## Status

Public Security Documentation

Version: 1.0

---

# Purpose

This document defines the trust boundaries used throughout the VRP architecture.

A trust boundary identifies where responsibility, authority and engineering assumptions change.

The objective is to describe observable trust relationships without exposing implementation details.

---

# Security Principle

Trust is never assumed.

Trust is established through observable runtime behavior.

Architectural correctness must not depend upon implicit trust.

---

# Primary Trust Domains

The VRP architecture separates responsibility into several trust domains.

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
      │
      ▼
External Network
```

Each layer has different responsibilities.

Trust does not automatically propagate between layers.

---

# Application Boundary

Applications are responsible for:

- business logic
- user interaction
- application state
- application policy

Applications do not control:

- authority evolution
- transport selection
- replay validation
- recovery policy

These responsibilities belong to the runtime.

---

# Runtime Boundary

The Protected Runtime is responsible for:

- Logical Session management
- Authority Epoch progression
- Runtime State Machine
- Replay Protection
- Recovery
- Multipath Selection
- Evidence generation

The runtime is the primary trust domain.

---

# Transport Boundary

Transport infrastructure is considered unreliable.

Examples include:

- Wi-Fi
- LTE
- 5G
- Ethernet
- relay infrastructure
- satellite links

Transport availability does not imply transport trust.

---

# External Network Boundary

External networks are treated as untrusted.

Observable failures may include:

- delay
- duplication
- reordering
- interruption
- instability

Runtime correctness must not depend upon trusted network behavior.

---

# Authority Boundary

Authority belongs to the Logical Session.

Authority never belongs to:

- transport
- infrastructure
- network interface
- routing path

Authority progression remains inside the runtime trust boundary.

---

# Evidence Boundary

Engineering evidence crosses the trust boundary.

Evidence is intentionally observable.

Protected implementation is not.

Evidence enables independent validation without implementation disclosure.

---

# Recovery Boundary

Recovery decisions remain inside the runtime.

Applications observe recovery.

Transport participates in recovery.

Recovery policy is never delegated to the transport layer.

---

# Administrative Boundary

Operational personnel may configure deployment.

They do not automatically obtain access to:

- implementation
- algorithms
- protected runtime internals
- proprietary engineering assets

Administrative authority is separate from implementation authority.

---

# Engineering Boundary

Public documentation defines:

- observable behavior
- engineering invariants
- architectural concepts
- validation methodology

Protected implementation defines:

- algorithms
- optimization
- scheduling
- synchronization
- internal runtime behavior

---

# Trust Assumptions

The architecture assumes:

- transport may fail
- infrastructure may restart
- communication may be delayed
- replay attempts may occur
- stale authority may appear

The runtime must preserve architectural correctness despite these assumptions.

---

# Trust Relationships

```
Application
      │
      │ Observable API
      ▼
Protected Runtime
      │
      │ Controlled Runtime Decisions
      ▼
Transport
      │
      │ Untrusted Communication
      ▼
External Network
```

Trust decreases as communication moves away from the runtime.

---

# Engineering Validation

Engineering evaluation should verify that trust boundaries remain intact during:

- replay attempts
- authority takeover
- recovery
- transport migration
- infrastructure restart
- concurrent execution

Observable behavior forms the basis of validation.

---

# Related Documents

THREAT_MODEL.md

ATTACK_TREE.md

SECURITY_MODEL.md

RUNTIME_BOUNDARIES.md

RFC-0009 — Security Boundary

ADR-0003 — Why the Runtime is Protected

---

# Summary

The VRP architecture defines explicit trust boundaries between applications, runtime, transport and external infrastructure.

Architectural correctness depends upon deterministic runtime behavior rather than implicit trust.

Protected implementation remains within the runtime trust boundary.

---

## Design Principle

Trust observable behavior.

Do not trust infrastructure.

Protect implementation.

Preserve deterministic execution.