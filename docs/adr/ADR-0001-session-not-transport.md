# ADR-0001 — Session ≠ Transport

**Status:** Accepted

**ADR Number:** 0001

**Date:** 2026

**Category:** Core Architecture

---

# Context

Traditional networking systems frequently bind application execution directly to a transport.

Examples include:

- TCP connections
- VPN tunnels
- IP addresses
- sockets
- network interfaces

When the transport changes, execution is often interrupted or restarted.

This coupling has historically been acceptable for relatively static networks.

Modern environments are different.

Devices continuously move between:

- Wi-Fi
- LTE
- 5G
- Ethernet
- relay infrastructure
- future transport technologies

The transport changes.

The application usually does not intend to.

---

# Problem

Treating transport as identity creates unnecessary coupling.

Observable consequences include:

- broken sessions
- reconnect storms
- duplicate execution
- authority conflicts
- inconsistent recovery
- application-visible interruptions

These outcomes reduce reliability.

---

# Decision

VRP defines the Logical Session as the primary execution object.

Transport becomes an implementation detail.

Applications communicate with a Logical Session.

The runtime manages transport evolution.

The application remains transport-independent.

---

# Decision Drivers

The following engineering objectives motivated this decision.

- continuity
- deterministic execution
- transport independence
- reproducible validation
- authority consistency
- replay resistance
- recovery correctness

Correctness takes precedence over transport persistence.

---

# Alternatives Considered

## Alternative A

Bind execution to transport.

Advantages:

- simpler implementation
- traditional networking model

Disadvantages:

- fragile mobility
- reconnect dependency
- transport-driven application failures

Rejected.

---

## Alternative B

Create a new session after every transport change.

Advantages:

- implementation simplicity

Disadvantages:

- loss of continuity
- duplicated initialization
- application-visible interruption
- increased recovery complexity

Rejected.

---

## Alternative C

Separate Session from Transport.

Advantages:

- continuity preserved
- deterministic runtime behavior
- transport evolution becomes transparent
- recovery simplified
- authority remains stable

Accepted.

---

# Consequences

Observable runtime behavior changes significantly.

Transport may evolve.

The Logical Session remains stable.

Recovery operates on the session.

Authority operates on the session.

Replay protection operates on the session.

Engineering validation focuses on session continuity rather than transport persistence.

---

# Benefits

The decision enables:

- transport independence
- deterministic recovery
- stable authority
- observable continuity
- engineering reproducibility
- simplified application model

---

# Trade-offs

The runtime becomes more sophisticated.

Additional coordination is required between:

- authority
- recovery
- transport management
- evidence generation

This additional complexity exists inside the protected runtime.

Applications become simpler.

---

# Architectural Impact

This decision directly influences:

RFC-0001 — Logical Session Identity

RFC-0002 — Authority Epochs

RFC-0003 — Transport Abstraction

RFC-0004 — Runtime State Machine

RFC-0007 — Failure Recovery

RFC-0008 — Multipath Selection

The decision is considered foundational.

---

# Validation

Observable validation includes:

- transport migration
- recovery
- replay rejection
- deterministic execution
- continuity preservation

Implementation remains protected.

---

# Status

This decision is permanent unless a future architecture demonstrates measurable engineering advantages without weakening continuity or deterministic behavior.

---

# Design Statement

The network may change.

The transport may change.

The infrastructure may change.

The Logical Session remains the same.

That separation is the foundation of the VRP architecture.