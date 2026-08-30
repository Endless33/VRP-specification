# Embedding the VRP Runtime

## Status

Public Integration Documentation

Version: 1.0

---

# Purpose

This document defines the architectural model for embedding the Veil Routing Protocol (VRP) Runtime into an existing software system.

The objective is to integrate continuity capabilities while minimizing application changes.

This document describes observable integration architecture.

Protected implementation details remain outside the scope of this specification.

---

# Engineering Philosophy

Applications should continue solving business problems.

The runtime should solve continuity problems.

Embedding VRP should require minimal architectural disruption.

Applications remain responsible for business logic.

The runtime becomes responsible for execution continuity.

---

# Integration Objectives

Embedding the runtime should provide:

- Logical Session continuity
- transport independence
- deterministic runtime behavior
- authority management
- recovery management
- observable engineering evidence

Applications should not need to implement these capabilities themselves.

---

# High-Level Architecture

```
Application

        │

        ▼

Application Adapter

        │

        ▼

VRP Runtime API

        │

        ▼

Protected VRP Runtime

        │

        ▼

Transport Infrastructure
```

The application communicates only through the Runtime API.

---

# Embedding Principle

The runtime should be embedded as a dedicated execution component.

Business logic remains outside the runtime.

Continuity logic remains inside the runtime.

Responsibilities remain clearly separated.

---

# Runtime Ownership

The embedded runtime owns:

- Logical Session lifecycle
- Authority progression
- Recovery decisions
- Replay protection
- Transport abstraction
- Engineering evidence

Applications observe runtime behavior.

They do not redefine runtime behavior.

---

# Application Responsibilities

Applications remain responsible for:

- business workflows
- user interaction
- domain logic
- persistence
- application configuration

Applications should not implement transport continuity.

---

# Runtime Responsibilities

The runtime manages:

- session continuity
- authority transitions
- transport migration
- recovery
- deterministic execution
- engineering evidence

These responsibilities remain implementation-defined.

---

# Transport Independence

Applications should not depend upon:

- IP addresses
- sockets
- network interfaces
- transport identifiers

Applications communicate through Logical Sessions.

Transport remains an implementation concern.

---

# Event Integration

Applications may observe runtime events including:

- session lifecycle
- authority changes
- recovery progress
- transport evolution
- evidence generation

Applications remain event consumers.

The runtime remains event producer.

---

# Failure Integration

Applications should allow the runtime to evaluate failures.

Applications should not attempt to override runtime recovery policy.

Failure handling remains centralized.

---

# Recovery Integration

Recovery is initiated by the runtime.

Applications receive observable recovery notifications.

Applications remain independent from recovery implementation.

---

# Security Boundary

Embedding the runtime does not require disclosure of:

- runtime source code
- internal algorithms
- transport scoring
- synchronization logic
- implementation heuristics

Observable behavior remains sufficient for engineering integration.

---

# Engineering Validation

Embedding should be validated by verifying:

- Logical Session continuity
- deterministic execution
- authority consistency
- replay rejection
- recovery correctness
- engineering evidence

Validation focuses on observable runtime behavior.

---

# Relationship to Other Documents

This document complements:

- API.md
- CALLBACKS.md
- EVENTS.md
- CONFIGURATION.md
- TRANSPORTS.md

It also supports:

- RFC-0010 — Runtime API
- RFC-0011 — Pilot Integration

---

# Summary

Embedding VRP introduces a continuity-focused runtime beneath the application layer.

Applications remain responsible for business logic.

The runtime remains responsible for continuity.

Observable engineering behavior remains independently verifiable.

---

## Design Principles

- Separate business logic from continuity.
- Embed the runtime once.
- Preserve the Logical Session.
- Centralize runtime decisions.
- Keep implementation protected.