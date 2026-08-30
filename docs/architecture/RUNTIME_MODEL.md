# Runtime Model

## Status

Public Architecture Specification

Version: 2.0

---

# Abstract

The VRP Runtime is responsible for preserving logical session continuity while continuously evaluating changing network conditions.

It operates as a deterministic decision layer positioned between the application and available transport paths.

The runtime does not assume that any transport is permanent.

Instead, it continuously observes runtime state, validates authority, evaluates available paths, and applies policy-driven decisions.

---

# Runtime Objectives

The runtime has four primary objectives.

- Preserve logical continuity whenever possible.
- Maintain deterministic behavior.
- Reject invalid state transitions.
- Produce observable and reproducible evidence.

Every runtime decision must satisfy these objectives.

---

# Runtime Responsibilities

The runtime is responsible for:

- session lifecycle management
- authority validation
- transport evaluation
- recovery coordination
- replay protection
- duplicate execution prevention
- evidence generation
- deterministic state transitions

These responsibilities remain active throughout the lifetime of every logical session.

---

# Observable Inputs

The runtime continuously evaluates observable information.

Examples include:

- transport availability
- transport health
- latency
- packet loss
- recovery progress
- authority state
- runtime events
- timing constraints

Observable inputs may change frequently.

Runtime behavior adapts without changing logical session identity whenever continuity remains valid.

---

# Runtime Decisions

The runtime evaluates observable state and produces deterministic decisions.

Examples include:

- continue current transport
- migrate transport
- reject stale authority
- reject replay attempts
- initiate recovery
- terminate execution
- preserve current state

Public documentation describes these outcomes.

Internal decision algorithms remain protected.

---

# Deterministic Behavior

Given equivalent observable conditions, the runtime is expected to produce equivalent externally observable behavior.

Determinism improves:

- reproducibility
- validation
- debugging
- operational confidence
- engineering analysis

The protected implementation may evolve while preserving externally observable behavior.

---

# Failure Handling

Failures are treated as expected operational events rather than exceptional situations.

Examples include:

- transport degradation
- transport loss
- temporary disconnects
- infrastructure failure
- mobility events
- authority conflicts

The runtime evaluates whether recovery remains possible according to runtime policy.

---

# Evidence

Every significant runtime decision may produce observable evidence.

Evidence enables independent validation without requiring access to protected implementation details.

Typical evidence categories include:

- validation results
- recovery events
- authority transitions
- runtime verdicts
- reproducible reports

---

# Protected Boundary

This document intentionally excludes:

- implementation details
- internal scheduling
- runtime heuristics
- protocol encoding
- cryptographic design
- proprietary optimization strategies

These remain part of the protected VRP runtime.

---

# Design Philosophy

Applications should interact with a stable logical runtime rather than individual transport technologies.

Transport evolves.

Runtime decisions evolve.

Logical continuity remains the architectural objective.

---

# Related Documents

- SESSION_NOT_TRANSPORT.md
- AUTHORITY_MODEL.md
- FAILURE_MODEL.md
- STATE_MACHINE.md
- RFC-0005-Runtime.md

---

> The runtime is responsible for decisions.
>
> The transport is responsible for delivery.
>
> The application is responsible for business logic.