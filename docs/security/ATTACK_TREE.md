# Attack Tree

## Status

Public Security Documentation

Version: 1.0

---

# Purpose

This document describes the observable attack surface considered during engineering validation of the Veil Routing Protocol (VRP).

Its purpose is to organize adversarial scenarios into a structured attack tree.

The document describes observable attack objectives rather than implementation defenses.

Implementation-specific protection mechanisms remain part of the protected runtime.

---

# Root Objective

Compromise deterministic runtime behavior.

Every observable attack can be viewed as an attempt to violate one or more architectural invariants.

```
Compromise Runtime
│
├── Compromise Session
├── Compromise Authority
├── Compromise Recovery
├── Compromise Transport
├── Compromise Evidence
└── Compromise Determinism
```

---

# Session Attacks

Objective:

Break Logical Session continuity.

Possible observable scenarios:

- forced reconnect
- session replacement
- duplicated session
- invalid session transition

Expected runtime behavior:

Maintain one Logical Session whenever architectural correctness permits.

---

# Authority Attacks

Objective:

Obtain unauthorized execution ownership.

Observable scenarios include:

- stale authority
- split-brain
- historical authority resurrection
- authority rollback
- concurrent ownership

Expected runtime behavior:

Reject non-canonical authority.

---

# Replay Attacks

Objective:

Reuse historical execution.

Observable scenarios:

- packet replay
- duplicated execution
- delayed execution
- historical runtime replay

Expected runtime behavior:

Reject replay.

---

# Recovery Attacks

Objective:

Abuse recovery to violate correctness.

Observable scenarios:

- recovery loop
- recovery rollback
- stale recovery
- invalid recovery order
- recovery after termination

Expected runtime behavior:

Recovery preserves architectural invariants or terminates safely.

---

# Transport Attacks

Objective:

Force incorrect transport behavior.

Observable scenarios:

- transport flapping
- transport exhaustion
- artificial degradation
- invalid transport transition
- repeated failover

Expected runtime behavior:

Logical Session remains independent from transport.

---

# State Machine Attacks

Objective:

Force impossible runtime state.

Observable scenarios:

- invalid transition
- skipped transition
- duplicated transition
- state resurrection

Expected runtime behavior:

Reject invalid transitions.

---

# Concurrency Attacks

Objective:

Exploit concurrent execution.

Observable scenarios:

- simultaneous authority acquisition
- concurrent recovery
- duplicate processing
- race conditions
- inconsistent ordering

Expected runtime behavior:

Deterministic outcome.

---

# Evidence Attacks

Objective:

Compromise engineering evidence.

Observable scenarios:

- evidence modification
- evidence truncation
- evidence forgery
- evidence replay
- inconsistent report generation

Expected runtime behavior:

Evidence integrity preserved.

---

# Resource Attacks

Objective:

Exhaust runtime resources.

Observable scenarios:

- excessive session creation
- transport storms
- replay floods
- event floods
- recovery storms

Expected runtime behavior:

Runtime remains architecturally correct under resource pressure.

---

# Composite Attacks

Complex attacks may combine multiple categories.

Examples include:

Replay

↓

Transport Failure

↓

Recovery

↓

Authority Takeover Attempt

↓

Evidence Verification

Observable runtime behavior should remain deterministic throughout the entire sequence.

---

# Engineering Validation

Representative validation includes:

- replay flood
- stale authority injection
- split-brain simulation
- transport migration
- transport storm
- recovery stress
- concurrency gauntlet
- evidence verification
- deterministic replay
- resource pressure

Observable behavior forms the basis of engineering evaluation.

---

# Security Philosophy

The runtime is not expected to prevent every failure.

The runtime is expected to preserve architectural correctness despite failure.

Correctness has priority over availability.

Deterministic execution has priority over undefined behavior.

---

# Related Documents

THREAT_MODEL.md

SECURITY_MODEL.md

TRUST_BOUNDARY.md

RFC-0002 — Authority Epochs

RFC-0006 — Replay Protection

RFC-0007 — Failure Recovery

RFC-0012 — Threat Model

---

# Summary

The Attack Tree organizes observable attack objectives into engineering categories.

Engineering validation demonstrates that runtime invariants remain preserved despite adversarial conditions.

Protected implementation determines how those objectives are achieved.

---

## Design Principle

Attack the runtime.

Observe the behavior.

Verify the invariants.