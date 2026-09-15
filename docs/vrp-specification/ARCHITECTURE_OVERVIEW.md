# VRP Architecture Overview

Document version: Public v1

Status: Public

---

# Purpose

This document summarizes the complete architectural model of VRP.

It serves as the entry point for engineers reading the public
specification.

Every detailed document referenced here expands one architectural area.

---

# Core Philosophy

VRP is not built around transport.

VRP is built around session continuity.

The central engineering principle is:

Session ≠ Transport

Transport may change.

Session remains canonical.

---

# Architectural Layers

The runtime consists of independent validation layers.

Session

↓

Lifetime

↓

Authority

↓

Lease

↓

State Machine

↓

Canonical Runtime

Each layer has one responsibility.

No layer replaces another.

---

# Canonical Identity

Canonical lifecycle identity is

(LifetimeEpoch, Incarnation)

This identity prevents:

- stale replay

- ABA

- restart rollback

- session resurrection

---

# Authority

Authority is validated separately.

Authority consists of:

- ownership

- authority generation

Authority never depends on transport.

---

# Lease

Lease coordinates distributed ownership.

Lease epoch is independent from:

- SessionID

- LifetimeEpoch

- Incarnation

- Authority Generation

---

# Canonical Runtime

Canonical runtime consists of:

- SessionManager

- AuthorityManager

- LeaseManager

- ControlPlane

- SessionRuntime

Only trusted components may mutate canonical state.

---

# Validation Pipeline

Incoming Event

↓

Trusted Producer

↓

Bootstrap Validation

↓

Lifetime Validation

↓

Authority Validation

↓

Lease Validation

↓

State Validation

↓

Canonical Event Log

↓

Transition History

↓

Canonical Runtime State

---

# Security Model

VRP protects against:

- replay

- stale authority

- stale lifetime

- duplicate transitions

- restart ABA

- unauthorized bootstrap

- malformed lifecycle identity

Every rejected operation fails closed.

---

# Canonical History

Only accepted transitions become canonical history.

Rejected events never modify:

- history

- event log

- runtime

- authority

- lifecycle

---

# Determinism

Canonical validation is deterministic.

Execution timing does not affect correctness.

Race execution does not affect correctness.

Shuffle execution does not affect correctness.

---

# Evidence

Engineering claims are supported by:

- regression tests

- race detector

- shuffle execution

- adversarial validation

- evidence bundles

- reproducible manifests

---

# Public Documentation

The public specification currently includes:

- Lifetime Identity Model

- Bootstrap Creation Authority

- ABA Protection Model

- Event Lifetime Binding

- Lifetime Security Invariants

- Trusted Event Delivery Model

- Canonical Session Manager

- Canonical Event Log

- Lifecycle Validation Pipeline

- Canonical Lifecycle State Machine

- Session Authority Model

- Lifecycle Boundary Model

- Canonical Event Acceptance Model

- Canonical Runtime Boundary

- Session Identity Model

- Canonical State Model

- Canonical History Model

- Canonical Evidence Model

- Fail-Closed Security Model

- Deterministic Validation Model

- Replay Protection Model

- Restart Recovery Model

- Verification and Evidence Model

- Engineering Principles

- Architecture Overview (this document)

---

# Design Principles

The complete architecture follows these rules:

✓ Validation before mutation

✓ Session ≠ Transport

✓ Authority is explicit

✓ Canonical state is unique

✓ History is immutable

✓ Replay is rejected

✓ Restart is restart-safe

✓ Bootstrap is privileged

✓ Evidence follows validation

✓ Fail closed

✓ Deterministic execution

---

# Final Statement

VRP is designed as a deterministic, fail-closed session continuity
runtime.

The architecture separates lifecycle, authority, lease, transport,
history, evidence, and runtime into independent validation domains.

Canonical runtime state exists only after successful validation.

Everything else is input.

Everything else is observation.

Everything else is evidence.