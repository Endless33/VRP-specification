# Canonical Evidence Model

Document version: Public v1

Status: Public

---

# Purpose

This document defines the evidence model used by VRP.

Evidence exists to explain and verify canonical runtime behavior.

Evidence is never authoritative.

Evidence never changes canonical runtime.

---

# Design Principle

Validation

↓

Canonical Mutation

↓

Evidence

Evidence is produced after canonical validation.

Evidence never participates in canonical decision making.

---

# Canonical Rule

Evidence may describe:

- accepted events

- rejected events

- validation failures

- runtime diagnostics

- security decisions

Evidence never creates authority.

Evidence never changes authority.

---

# Canonical Evidence

Canonical evidence is generated from accepted runtime behavior.

Examples:

- canonical transition

- authority transfer

- lease renewal

- migration

- recovery

---

# Security Evidence

Security evidence records rejected operations.

Examples:

- replay

- stale lifetime

- stale authority

- stale lease

- invalid transition

- duplicate event

- unauthorized bootstrap

Rejected operations become evidence.

They never become runtime state.

---

# Evidence Flow

Incoming Event

↓

Validation

↓

Accepted

↓

Canonical Runtime

↓

Evidence

or

Incoming Event

↓

Validation

↓

Rejected

↓

Security Evidence

---

# Canonical Properties

Evidence is:

- append-only

- deterministic

- reproducible

- exportable

- verifiable

---

# Evidence Independence

Evidence must never mutate:

- SessionManager

- Event Log

- Transition History

- Authority

- Lease

- Runtime State

Evidence is observational only.

---

# Export

Evidence may be exported for:

- validation

- audit

- pilot verification

- engineering analysis

Export never modifies runtime.

---

# Reproducibility

The same execution produces equivalent evidence.

Independent verification should produce the same conclusions.

---

# Security Guarantees

Evidence guarantees:

✓ deterministic recording

✓ immutable export

✓ replay diagnostics

✓ stale lifecycle diagnostics

✓ restart diagnostics

✓ fail-closed reporting

---

# Relationship to Canonical State

Canonical State

↓

Evidence

The reverse direction never exists.

Evidence cannot reconstruct authority.

Evidence cannot create authority.

Evidence cannot replace validation.

---

# Design Principle

Evidence explains what happened.

Validation decides what is allowed.

Only validation changes canonical runtime.