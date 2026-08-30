# Changelog

All notable architectural and engineering changes to the Veil Routing Protocol (VRP) are documented in this file.

The project follows the versioning policy defined in:

engineering/VERSIONING.md

---

# Guiding Principles

The changelog records:

- architectural evolution
- engineering milestones
- runtime improvements
- specification updates
- compatibility changes

Implementation-specific development activity is intentionally excluded unless it affects observable architecture.

---

# Version 1.0.0

## Status

Initial Public Engineering Specification

---

## Added

### Architecture

- Logical Session architecture
- Session ≠ Transport model
- Runtime architecture
- Authority model
- Failure model
- Security boundary

---

### RFC Series

Initial publication of the public RFC specification.

Including:

- Logical Session Identity
- Authority Epochs
- Transport Abstraction
- Runtime State Machine
- Evidence Model
- Replay Protection
- Failure Recovery
- Multipath Selection
- Security Boundary
- Runtime API
- Pilot Integration
- Threat Model

---

### ADR Series

Published architectural decisions:

- Session Before Transport
- Authority Epochs
- Protected Runtime
- Evidence-First Validation
- Black-Box Evaluation
- Why VRP Is Not a VPN

---

### Runtime Documentation

Added documentation for:

- Runtime Invariants
- Runtime State Machine
- Runtime Events
- Authority Transitions
- Failure Handling
- Recovery Rules

---

### Security Documentation

Added documentation for:

- Threat Model
- Attack Tree
- Trust Boundary
- Security Model
- Operator Trust Model
- Runtime Boundaries

---

### Evaluation Documentation

Added documentation for:

- Test Matrix
- PASS Criteria
- FAILURE Criteria
- Reproducibility
- Evidence Format
- Audit Guide

---

### Integration Documentation

Added documentation for:

- Runtime Embedding
- Runtime API
- Callbacks
- Runtime Events
- Runtime Configuration
- Transport Abstraction

---

### Engineering Documentation

Published:

- Design Principles
- Roadmap
- Versioning Policy

---

## Security

Defined public security architecture including:

- replay protection model
- authority model
- runtime trust boundaries
- protected implementation boundary
- engineering validation methodology

---

## Validation

Established public validation methodology based upon:

- observable runtime behavior
- deterministic execution
- engineering evidence
- independent verification

---

## Compatibility

Initial public specification.

No previous version exists.

---

# Future Releases

Future versions will document changes using the following categories.

## Added

New architectural capabilities.

---

## Changed

Observable architectural evolution.

---

## Deprecated

Concepts scheduled for removal.

---

## Removed

Architectural concepts no longer supported.

---

## Fixed

Architectural corrections.

---

## Security

Security improvements affecting observable runtime behavior.

---

# Changelog Policy

The changelog records architectural evolution.

It is not intended to document every implementation commit.

Observable engineering behavior has priority over implementation history.

---

# Related Documents

- VERSIONING.md
- ROADMAP.md
- DESIGN_PRINCIPLES.md

---

# Summary

The changelog provides a stable historical record of the evolution of the VRP architecture.

Engineering history should remain understandable long after individual implementations evolve.

---

## Design Principles

- Document architectural evolution.
- Preserve engineering history.
- Communicate compatibility.
- Record observable changes.
- Keep implementation details separate.