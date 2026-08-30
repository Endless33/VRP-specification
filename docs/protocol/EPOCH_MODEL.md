# Epoch Model

## Status

Public Protocol Specification

Version: 2.0

---

# Abstract

The Epoch Model defines how observable authority evolves during the lifetime of a logical session.

Its primary objective is to provide deterministic progression of authority while preventing ambiguity, stale ownership, replay acceptance, and conflicting execution.

This document describes the observable architectural model.

Implementation details remain part of the protected VRP runtime.

---

# Purpose

Epochs provide an observable ordering mechanism for authority evolution.

Instead of relying on transport identity or infrastructure topology, the runtime evaluates authority progression according to monotonic runtime evolution.

Epochs are associated with authority evolution rather than transport replacement.

---

# Design Objectives

The Epoch Model is designed to provide:

- deterministic authority evolution
- monotonic progression
- observable ordering
- replay resistance
- stale authority rejection
- engineering reproducibility

---

# Observable Properties

Every observable authority state belongs to one observable epoch.

An epoch represents the current generation of canonical authority for a logical session.

Only one observable epoch is considered canonical at any point in time.

---

# Monotonic Progression

Epoch evolution always progresses forward.

Observable runtime behavior never intentionally returns to a previously superseded epoch.

This property simplifies:

- validation
- debugging
- engineering analysis
- distributed recovery

---

# Epoch Independence

Epochs are independent from:

- IP addresses
- transport technologies
- network interfaces
- infrastructure placement
- physical topology

Changing transport does not automatically create a new epoch.

Likewise, changing an epoch does not necessarily require transport replacement.

---

# Epoch Evolution

Observable epoch evolution may occur during:

- controlled authority transfer
- infrastructure failover
- runtime restart
- recovery
- controlled migration

The protected runtime determines when epoch evolution is required.

---

# Canonical Epoch

At every observable point in time:

- one logical session
- one canonical authority
- one canonical epoch

The runtime prevents conflicting observable authority generations.

---

# Stale Epochs

Historical epochs remain part of observable history.

They are not considered authoritative.

Previously superseded epochs must not silently become canonical again.

Observable validation may report:

- stale epoch rejected
- stale authority rejected
- canonical authority preserved

---

# Replay Protection

Previously accepted epoch transitions must not be accepted again solely because they are observed a second time.

Replay protection remains active throughout runtime execution.

Replay rejection is independent from transport changes.

---

# Epoch Validation

Before accepting observable authority evolution, the runtime validates:

- current canonical authority
- runtime consistency
- observable session state
- policy requirements

Only validated epoch transitions become observable runtime state.

---

# Recovery

Recovery may preserve the current epoch.

Recovery may also introduce a new canonical epoch.

The decision depends on runtime policy rather than transport behavior.

Observable recovery does not imply observable epoch evolution.

---

# Engineering Validation

Independent engineering teams may verify:

- monotonic epoch progression
- deterministic authority evolution
- stale epoch rejection
- replay resistance
- observable continuity

Protected implementation remains outside the scope of evaluation.

---

# Protected Boundary

This document intentionally excludes:

- epoch numbering algorithms
- internal authority synchronization
- implementation-specific transition logic
- runtime coordination mechanisms
- protocol encoding
- proprietary optimization strategies

These remain part of the protected VRP runtime.

---

# Related Documents

- AUTHORITY_MODEL.md
- AUTHORITY_TRANSITIONS.md
- SESSION_LIFECYCLE.md
- INVARIANTS.md
- RFC-0002-Authority-Epochs.md

---

# Summary

Epochs describe observable authority evolution.

They do not describe transport behavior.

They do not describe implementation mechanisms.

Their purpose is to provide deterministic, monotonic and reproducible authority progression throughout the lifetime of a logical session.

---

> Authority evolves.

> Epochs order that evolution.

> Determinism makes the evolution observable.