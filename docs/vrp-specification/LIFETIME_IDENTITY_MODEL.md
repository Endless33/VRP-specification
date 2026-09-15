# VRP Lifetime Identity Model

Document version: Public v1

## Purpose

This document defines canonical session lifetime identity inside VRP.

The goal is to prevent stale session resurrection while preserving deterministic lifecycle validation.

---

# Canonical Identity

Canonical session identity is defined as:

(LifetimeEpoch, Incarnation)

Neither component alone is sufficient.

---

## LifetimeEpoch

LifetimeEpoch identifies one SessionManager lifetime namespace.

Properties:

- generated once during SessionManager creation
- unpredictable
- non-zero
- never reused intentionally
- independent from authority generation
- independent from lease epoch

---

## Incarnation

Incarnation identifies one canonical lifetime inside one LifetimeEpoch.

Properties:

- monotonic
- starts from 1
- local to one SessionManager lifetime
- reused only together with different LifetimeEpoch

---

## Security Properties

Rejected:

- stale events
- delayed events
- replay across lifetime
- ABA resurrection
- restart rollback

Preserved:

- deterministic validation
- fail closed
- canonical history