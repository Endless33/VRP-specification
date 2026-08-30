# Runtime Callbacks

## Status

Public Integration Documentation

Version: 1.0

---

# Purpose

This document defines the architectural callback model of the Veil Routing Protocol (VRP).

Callbacks provide an observable mechanism through which applications receive runtime notifications.

Callbacks communicate runtime events.

They do not control runtime decisions.

Implementation-specific callback interfaces remain outside the scope of this specification.

---

# Engineering Philosophy

The runtime owns execution.

Applications observe execution.

Callbacks exist to notify applications about significant runtime events without exposing protected implementation.

Applications remain reactive.

The runtime remains authoritative.

---

# Callback Objectives

The callback model is designed to provide:

- deterministic event notification
- transport-independent communication
- observable runtime state
- minimal application coupling
- stable integration

Callbacks communicate architectural state.

---

# Architectural Position

```
Application

        ▲

        │

Callbacks

        ▲

        │

Protected Runtime

        │

        ▼

Transport Infrastructure
```

Applications never invoke callbacks directly.

The runtime produces callbacks.

---

# Callback Categories

The runtime may generate callbacks for:

- Session lifecycle
- Authority changes
- Transport evolution
- Recovery lifecycle
- Runtime lifecycle
- Security events
- Evidence availability

Each callback corresponds to an observable runtime event.

---

# Session Callbacks

Representative callbacks include:

- SessionCreated
- SessionActivated
- SessionUpdated
- SessionClosed

Applications may update business logic in response.

Session ownership remains unchanged.

---

# Authority Callbacks

Representative callbacks include:

- AuthorityEstablished
- AuthorityConfirmed
- AuthorityChanged
- AuthorityRejected

Applications observe authority evolution.

Authority decisions remain internal.

---

# Transport Callbacks

Representative callbacks include:

- TransportSelected
- TransportChanged
- TransportRecovered
- TransportUnavailable

Applications remain transport-independent.

Transport events are informational.

---

# Recovery Callbacks

Representative callbacks include:

- RecoveryStarted
- RecoveryProgress
- RecoveryCompleted
- RecoveryFailed

Recovery policy remains controlled by the runtime.

Applications receive observable status updates.

---

# Runtime Callbacks

Representative callbacks include:

- RuntimeInitialized
- RuntimeReady
- RuntimeStopping
- RuntimeStopped

These callbacks describe runtime lifecycle progression.

---

# Security Callbacks

Representative callbacks include:

- ReplayRejected
- DuplicateRejected
- InvalidTransitionRejected
- StaleAuthorityRejected

Security callbacks document preservation of architectural invariants.

---

# Evidence Callbacks

Representative callbacks include:

- EvidenceAvailable
- EvidenceVerified
- ValidationCompleted

Applications may archive or process observable engineering evidence.

Evidence generation remains internal.

---

# Callback Ordering

Callbacks should follow deterministic ordering.

Equivalent runtime execution should produce equivalent callback sequences.

Applications should never depend upon undefined callback ordering.

---

# Callback Reliability

Applications should assume:

- callbacks may be asynchronous;
- callbacks may be delayed by infrastructure;
- callback delivery mechanisms are implementation-specific.

Architectural meaning remains unchanged.

---

# Callback Responsibilities

Applications SHOULD:

- observe callback events;
- update application state;
- log significant events;
- initiate business workflows when appropriate.

Applications SHOULD NOT:

- redefine runtime decisions;
- override authority;
- modify recovery behavior;
- bypass runtime policy.

---

# Engineering Validation

Validation should verify:

- callback ordering;
- deterministic callback generation;
- callback consistency;
- replay-safe notification;
- observable event correspondence.

Callbacks should accurately reflect runtime behavior.

---

# Relationship to Other Documents

This document complements:

- EMBEDDING.md
- API.md
- EVENTS.md
- CONFIGURATION.md
- TRANSPORTS.md

It also supports:

- RFC-0010 — Runtime API
- RFC-0011 — Pilot Integration

---

# Summary

Callbacks provide the observable notification layer between the Protected VRP Runtime and the application.

Applications observe runtime evolution through callbacks.

The runtime remains solely responsible for execution decisions.

---

## Design Principles

- The runtime decides.
- Applications observe.
- Callbacks communicate.
- Behavior remains deterministic.
- Implementation remains protected.