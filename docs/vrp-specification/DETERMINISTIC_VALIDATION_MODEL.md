# Deterministic Validation Model

Document version: Public v1

Status: Public

---

# Purpose

This document defines the deterministic validation model used by VRP.

The same canonical input must always produce the same canonical output.

Validation must never depend on:

- execution timing

- scheduler order

- CPU load

- thread ordering

- transport timing

- packet arrival timing

---

# Deterministic Rule

Canonical Input

↓

Canonical Validation

↓

Canonical Result

Every execution must produce the same result.

---

# Canonical Inputs

Validation considers only canonical information:

- SessionID

- LifetimeEpoch

- Incarnation

- Authority

- Lease

- Current State

- Event

No external timing information changes validation.

---

# Non-Canonical Inputs

The following never determine canonical validity:

- latency

- RTT

- jitter

- CPU speed

- packet ordering

- operating system scheduling

- goroutine scheduling

- transport selection

These values may affect performance.

They never affect correctness.

---

# Validation Pipeline

Incoming Event

↓

Lifecycle Validation

↓

Authority Validation

↓

Lease Validation

↓

State Validation

↓

Canonical Decision

The order never changes.

---

# Accepted Event

Accepted events always produce:

- identical canonical state

- identical transition

- identical event log

- identical history

for identical canonical inputs.

---

# Rejected Event

Rejected events always produce:

- identical rejection

- identical canonical state

- identical history

- identical event log

No partial mutation is permitted.

---

# Concurrency

Parallel execution must preserve deterministic validation.

Running with:

- one thread

or

- many threads

must produce the same canonical result.

---

# Race Safety

Race detection must not change runtime semantics.

Running with:

go test

and

go test -race

must preserve canonical behavior.

---

# Shuffle Safety

Random execution order must not affect canonical validation.

Running:

go test -shuffle

must preserve:

- state

- history

- authority

- lifecycle

---

# Evidence

Evidence may contain timestamps.

Canonical validation never depends on timestamps.

Evidence records results.

Evidence does not create results.

---

# Security Guarantees

Deterministic validation guarantees:

✓ identical canonical decisions

✓ replay resistance

✓ deterministic history

✓ deterministic authority

✓ deterministic lifecycle validation

✓ deterministic restart behavior

✓ deterministic event ordering

---

# Design Principle

Performance may vary.

Execution timing may vary.

Transport may vary.

Canonical validation never varies.