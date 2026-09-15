# Replay Protection Model

Document version: Public v1

Status: Public

---

# Purpose

This document defines the replay protection architecture used by VRP.

Replay protection is not implemented by a single mechanism.

Instead, replay resistance is achieved through multiple independent
validation layers.

An attacker must bypass every layer simultaneously.

---

# Design Principle

Replay protection is fail-closed.

If replay cannot be disproven,

the operation is rejected.

---

# Replay Layers

VRP replay protection consists of:

- Session Identity

- Lifetime Identity

- Authority Validation

- Lease Validation

- State Validation

- Canonical History

Each layer protects a different attack surface.

---

# Layer 1 — Session Identity

SessionID determines the logical communication session.

Packets from another SessionID
never become part of the current session.

---

# Layer 2 — Lifetime Identity

Canonical lifetime is

(LifetimeEpoch, Incarnation)

Old lifetime

↓

Current lifetime

↓

Rejected

A replay from a previous lifetime is never accepted.

---

# Layer 3 — Authority

Replay cannot recreate authority.

Current authority must validate successfully.

Old authority is rejected.

---

# Layer 4 — Lease

Replay cannot recreate ownership.

Lease validation ensures:

- owner

- epoch

- lease

remain current.

---

# Layer 5 — State Machine

Even if replay passes previous layers,

the event must still be valid for the current canonical state.

Invalid transitions are rejected.

---

# Layer 6 — Canonical History

Accepted canonical transitions are immutable.

Replay never rewrites history.

Replay never inserts duplicate history.

---

# Replay Sources

Examples include:

- duplicated packets

- delayed packets

- delayed migration

- delayed recovery

- delayed ownership transfer

- duplicated transport events

- stale runtime messages

- stale lifecycle events

---

# Restart Replay

Manager A

↓

Restart

↓

Manager B

↓

Old replay

↓

Rejected

Lifetime namespace prevents replay across restart.

---

# Same-Manager Replay

Lifetime A

↓

Destroy

↓

Lifetime B

↓

Old replay

↓

Rejected

Incarnation prevents replay inside one manager lifetime.

---

# Canonical Pipeline

Incoming Event

↓

Lifetime Validation

↓

Authority Validation

↓

Lease Validation

↓

State Validation

↓

Canonical Mutation

Replay is rejected before mutation.

---

# Security Guarantees

Replay protection guarantees:

✓ stale lifetime rejection

✓ stale authority rejection

✓ stale lease rejection

✓ duplicate transition rejection

✓ immutable history

✓ restart-safe replay protection

✓ deterministic validation

✓ fail-closed execution

---

# Design Principle

Replay is never evaluated by transport alone.

Replay is evaluated against the complete canonical runtime state.

Only events belonging to the current canonical lifecycle may mutate runtime.