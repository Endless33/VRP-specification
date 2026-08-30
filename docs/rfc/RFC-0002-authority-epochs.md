# RFC-0002 — Authority Epochs

**Document Status:** Public Specification

**RFC Number:** RFC-0002

**Version:** 1.0

**Category:** Core Architecture

---

# Abstract

This document defines the observable Authority Epoch model used by the VRP Runtime.

Authority Epochs provide deterministic ordering of authority evolution throughout the lifetime of a Logical Session.

Their purpose is to ensure that authority progresses monotonically while preventing stale ownership, conflicting execution and ambiguous recovery.

Implementation mechanisms remain part of the protected VRP Runtime.

---

# 1. Introduction

Distributed systems continuously experience:

- failover
- recovery
- transport migration
- infrastructure restart
- temporary partition
- authority transfer

Without deterministic ordering, multiple authorities may simultaneously believe they are canonical.

The Authority Epoch model prevents this ambiguity.

---

# 2. Terminology

The key words **MUST**, **MUST NOT**, **SHOULD**, **SHOULD NOT**, and **MAY** in this document are to be interpreted as described in RFC 2119.

---

# 3. Authority Epoch

An Authority Epoch represents one observable generation of canonical authority.

Each Logical Session MUST have exactly one canonical Authority Epoch at any observable point in time.

---

# 4. Monotonicity

Authority Epochs MUST progress monotonically.

A runtime MUST NOT intentionally transition to an earlier observable epoch.

Historical epochs remain observable history only.

---

# 5. Canonical Authority

Exactly one authority MAY be considered canonical.

The runtime MUST reject observable situations where multiple authorities simultaneously claim ownership of the same Logical Session.

---

# 6. Epoch Evolution

Authority Epochs MAY evolve because of:

- controlled failover
- infrastructure recovery
- runtime restart
- authority migration
- operational policy

Transport replacement alone MUST NOT require Authority Epoch evolution.

---

# 7. Stale Authority

Previously superseded authority MUST NOT automatically become canonical.

Observable stale authority MUST be rejected.

Examples include:

- delayed execution
- resurrected infrastructure
- outdated ownership
- obsolete runtime state

---

# 8. Replay Independence

Replay Protection and Authority Epochs address different architectural concerns.

Replay Protection validates execution freshness.

Authority Epochs validate ownership progression.

Both mechanisms MUST remain independent.

---

# 9. Recovery

Recovery MAY preserve the current Authority Epoch.

Recovery MAY establish a newer Authority Epoch.

Recovery MUST NOT restore a superseded Authority Epoch.

---

# 10. Observable Validation

Independent engineering validation SHOULD be able to observe:

- monotonic authority progression
- stale authority rejection
- deterministic authority evolution
- reproducible runtime behavior

Implementation disclosure is not required.

---

# 11. Engineering Invariants

The runtime MUST preserve the following observable invariants:

- one canonical authority
- monotonic epoch progression
- deterministic authority evolution
- stale authority rejection
- reproducible runtime behavior

Violation of these invariants represents observable runtime failure.

---

# 12. Security Considerations

The Authority Epoch model strengthens:

- split-brain resistance
- replay resilience
- recovery correctness
- deterministic execution
- runtime integrity

Internal implementation remains protected.

---

# 13. Non-Goals

This RFC does not define:

- epoch numbering algorithms
- synchronization protocols
- implementation heuristics
- scheduling algorithms
- packet encoding

These remain proprietary implementation details.

---

# 14. Related RFCs

RFC-0001 — Logical Session Identity

RFC-0003 — Transport Abstraction

RFC-0006 — Replay Protection

RFC-0007 — Failure Recovery

---

# 15. Summary

Authority Epochs provide deterministic ordering of canonical ownership.

Observable authority progresses forward.

Historical authority remains historical.

Engineering validation focuses on observable monotonic behavior rather than implementation.

---

## Normative Requirements

- A Logical Session **MUST** have one canonical authority.
- Authority Epochs **MUST** progress monotonically.
- Historical epochs **MUST NOT** become canonical again.
- Recovery **MUST NOT** violate epoch ordering.
- Observable behavior **SHOULD** remain deterministic.

---

## Design Principle

> Authority progresses.

> Epochs order that progression.

> Deterministic ordering preserves trust.