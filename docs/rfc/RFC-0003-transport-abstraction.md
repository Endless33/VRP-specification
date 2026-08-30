# RFC-0003 — Transport Abstraction

**Document Status:** Public Specification

**RFC Number:** RFC-0003

**Version:** 1.0

**Category:** Core Architecture

---

# Abstract

This document defines the Transport Abstraction model of the Veil Routing Protocol (VRP).

Transport Abstraction separates communication continuity from the physical transport carrying application traffic.

A Logical Session remains stable while the underlying transport infrastructure evolves.

Implementation mechanisms remain part of the protected VRP Runtime.

---

# 1. Introduction

Modern communication systems operate across dynamic networks.

During normal execution, transports may change because of:

- Wi-Fi availability
- mobile network transitions
- Ethernet changes
- routing updates
- relay selection
- infrastructure migration
- temporary failures

Traditional systems often associate execution with one transport.

VRP intentionally separates execution continuity from transport selection.

---

# 2. Terminology

The key words **MUST**, **MUST NOT**, **SHOULD**, **SHOULD NOT**, and **MAY** in this document are to be interpreted as described in RFC 2119.

---

# 3. Definition

Transport Abstraction is the architectural separation between:

- Logical Session Identity
- transport implementation

A transport carries execution.

A transport does not define execution.

---

# 4. Architectural Model

```
Application
      │
      ▼
Logical Session
      │
      ▼
Authority Layer
      │
      ▼
Transport Abstraction
      │
      ▼
Selected Transport
      │
      ▼
Network
```

The runtime owns transport selection.

Applications remain transport-independent.

---

# 5. Observable Properties

Transport Abstraction provides observable properties including:

- transport independence
- deterministic transition
- continuity preservation
- runtime observability
- engineering reproducibility

Implementation remains protected.

---

# 6. Transport Evolution

A transport MAY change during execution.

Examples include:

Wi-Fi

↓

LTE

↓

5G

↓

Satellite

↓

Relay

↓

Ethernet

The Logical Session remains unchanged.

---

# 7. Selection

The runtime evaluates available transports according to internal policy.

Selection mechanisms remain proprietary.

Observable transport evolution SHOULD remain deterministic for equivalent runtime conditions.

---

# 8. Recovery

Transport recovery MAY involve:

- returning to a previous transport
- selecting an alternative transport
- remaining on the current transport

Recovery decisions remain runtime-controlled.

Applications observe the result rather than the algorithm.

---

# 9. Failure

Transport failures do not necessarily terminate execution.

The runtime evaluates whether continuity can be preserved.

Observable recovery behavior is preferred whenever engineering invariants remain satisfied.

---

# 10. Independence

Applications SHOULD NOT depend upon:

- transport identifiers
- network interfaces
- IP addresses
- routing topology
- transport implementation

Applications communicate through the Logical Session.

---

# 11. Engineering Validation

Observable validation MAY include:

- transport migration
- failover
- recovery
- deterministic selection
- runtime continuity
- evidence generation

Evaluation focuses on observable behavior.

---

# 12. Engineering Invariants

The runtime MUST preserve:

- Logical Session identity
- canonical authority
- deterministic behavior
- replay protection
- transport independence

Transport evolution MUST NOT violate these observable invariants.

---

# 13. Security Considerations

Transport Abstraction reduces coupling between execution and infrastructure.

Observable benefits include:

- infrastructure flexibility
- deterministic recovery
- resilience to transport disruption
- consistent runtime behavior

Implementation details remain protected.

---

# 14. Non-Goals

This RFC does not define:

- routing algorithms
- transport scoring
- transport scheduling
- network discovery
- optimization heuristics
- protocol encoding

These remain implementation-specific.

---

# 15. Related RFCs

RFC-0001 — Logical Session Identity

RFC-0002 — Authority Epochs

RFC-0004 — Runtime State Machine

RFC-0008 — Multipath Selection

---

# 16. Summary

Transport Abstraction separates execution continuity from network infrastructure.

The runtime owns transport evolution.

Applications observe continuity.

Observable engineering behavior remains reproducible while implementation remains protected.

---

## Normative Requirements

- Applications **MUST NOT** depend on transport identity.
- Transport evolution **MUST NOT** redefine the Logical Session.
- Runtime behavior **SHOULD** remain deterministic.
- Observable continuity **MUST** preserve architectural invariants.
- Transport implementation **MAY** evolve independently.

---

## Design Principle

> Transport carries execution.

> Transport never becomes execution.

> Continuity belongs to the Logical Session.