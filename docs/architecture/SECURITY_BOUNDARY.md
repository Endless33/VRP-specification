# Security Boundary

## Status

Public Architecture Specification

Version: 2.0

---

# Abstract

The Security Boundary defines the separation between information intentionally disclosed for public engineering evaluation and information that remains part of the protected VRP implementation.

The objective is to enable meaningful technical evaluation without requiring disclosure of proprietary runtime mechanisms.

This separation is considered an architectural design principle rather than an operational restriction.

---

# Design Objectives

The security boundary is intended to:

- enable independent engineering evaluation
- protect proprietary implementation
- preserve reproducibility
- support enterprise technical review
- reduce unnecessary information exposure
- maintain long-term protocol evolution

---

# Public Domain

The following categories are intentionally public.

## Architecture

High-level architectural concepts.

Examples include:

- continuity-first design
- session independence
- transport abstraction
- authority model
- runtime responsibilities
- recovery philosophy

---

## Runtime Behavior

Observable runtime behavior may be documented.

Examples include:

- transport migration
- recovery
- authority transitions
- replay rejection
- evidence generation
- deterministic outcomes

Behavior may be validated without exposing implementation.

---

## Engineering Documentation

Public documentation includes:

- architecture
- RFC
- ADR
- diagrams
- evaluation guides
- integration guides
- operational concepts

---

## Validation

Public validation may demonstrate:

- reproducible tests
- observable runtime behavior
- stress testing
- adversarial scenarios
- evidence verification
- engineering reports

Validation focuses on externally observable properties.

---

# Protected Domain

The following information is intentionally excluded from public documentation.

## Runtime Implementation

- production runtime source code
- internal modules
- implementation details
- scheduling logic
- optimization techniques

---

## Protocol Internals

Protected protocol mechanisms include, but are not limited to:

- internal packet encoding
- protected state transitions
- implementation-specific message formats
- runtime synchronization logic
- proprietary protocol evolution

---

## Security Mechanisms

The protected runtime may contain mechanisms whose implementation remains confidential.

Examples include:

- internal verification logic
- protected recovery mechanisms
- implementation-specific validation
- runtime integrity mechanisms

Public documentation describes objectives rather than implementation.

---

## Operational Assets

The following are never distributed through public documentation.

- deployment credentials
- production keys
- confidential infrastructure
- operational secrets
- customer environments
- private evaluation artifacts

---

# Evaluation Philosophy

VRP is designed to be evaluated through observable behavior.

Independent reviewers should be able to validate:

- deterministic execution
- recovery behavior
- authority preservation
- replay rejection
- evidence integrity

without requiring access to protected implementation.

---

# Engineering Review

Enterprise technical evaluation may include:

- documentation review
- architecture review
- reproducible testing
- runtime observation
- evidence verification

Implementation disclosure is not required.

---

# Intellectual Property

Publication of this repository does not transfer:

- ownership
- implementation rights
- runtime source code
- proprietary algorithms
- confidential engineering knowledge

Intellectual property remains protected.

---

# Future Evolution

The protected runtime may evolve independently from the public specification.

Future implementation improvements are not required to preserve internal structure.

Only externally documented architectural guarantees are expected to remain stable unless explicitly revised.

---

# Related Documents

- RUNTIME_MODEL.md
- THREAT_MODEL.md
- EVALUATION_PROCESS.md
- EVALUATION_LICENSE.md
- TRADE_SECRET_NOTICE.md

---

> Public documentation explains observable engineering.

> Protected implementation explains how that engineering is achieved.