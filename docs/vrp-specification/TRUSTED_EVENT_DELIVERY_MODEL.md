# Trusted Event Delivery Model

Document version: Public v1

Status: Public

---

# Purpose

This document defines which VRP components are allowed to produce canonical
state transitions.

Canonical state must never be modified by arbitrary event injection.

---

# Canonical Rule

Only trusted producers may deliver canonical events.

Every trusted event is bound to the current session lifetime before entering
SessionManager.

---

# Trusted Producers

Current trusted producers are:

- SessionRuntime
- ControlPlane

These components operate inside the protected runtime.

---

# Generic Event Delivery

Generic event delivery is NOT a trusted producer.

Receiving an Event object does not imply authority.

The SessionManager validates lifecycle identity before accepting any state
transition.

---

# Bootstrap Exception

Session creation is a special case.

No canonical lifetime exists before CreateSession.

Therefore bootstrap is performed only by trusted creation authority.

Bootstrap is not ordinary event delivery.

---

# Event Flow

Trusted component

↓

Create event

↓

Bind current LifetimeEpoch

↓

Bind current Incarnation

↓

SessionManager validation

↓

Canonical event log

↓

State transition

---

# Security Guarantees

Trusted producers:

✓ canonical history

✓ canonical state

✓ deterministic validation

Untrusted producers:

✗ cannot bootstrap sessions

✗ cannot mutate canonical history

✗ cannot bypass lifecycle validation

---

# Security Objective

Authority must be explicit.

Trust must never be inferred from the existence of an Event object.