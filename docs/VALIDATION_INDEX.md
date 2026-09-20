# VRP Validation Index

## Purpose

This document serves as the entry point for the public engineering validation package.

It summarizes the observable engineering documentation currently available for VRP.

The objective is to provide engineers with a structured path through the public architectural material without exposing protected runtime implementation.

---

# Documentation Structure

## Architecture

- ARCHITECTURAL_GUARANTEES.md

Describes the observable architectural guarantees expected from VRP.

---

## Validation Philosophy

- VALIDATION_PHILOSOPHY.md

Explains how engineering claims are evaluated and why reproducible evidence is treated as a first-class engineering requirement.

---

## Validation Scope

- VALIDATION_SCOPE.md

Defines the observable engineering areas currently covered by public validation.

---

## Verified Engineering Properties

- VERIFIED_ENGINEERING_PROPERTIES.md

Summarizes engineering properties that have already been demonstrated through reproducible validation.

---

## Failure Model

- FAILURE_MODEL.md

Describes how failures are classified and how observable correctness is preserved under adverse conditions.

---

## Threat Model

- THREAT_MODEL.md

Defines the classes of failures and adversarial scenarios considered during public engineering validation.

---

## Engineering Evidence

- ENGINEERING_EVIDENCE.md

Explains what constitutes engineering evidence within VRP and how engineering conclusions are supported.

---

# Validation Principles

The public validation program is built around several engineering principles.

- deterministic execution

- reproducible validation

- adversarial testing

- observable runtime behavior

- engineering evidence

- fail-closed behavior

- independent verification

Observable engineering evidence is considered stronger than architectural assertion.

---

# Observable Validation Areas

Current public engineering validation includes observable evidence covering:

- deterministic runtime execution

- transport migration

- session continuity

- replay rejection

- stale authority rejection

- duplicate execution rejection

- authority validation

- canonical state preservation

- canonical history preservation

- restart boundaries

- session reincarnation

- concurrent execution

- randomized execution ordering

- race detection

- deterministic repetition

- resource-bounded execution

- evidence integrity

---

# Public Engineering Boundary

The public documentation intentionally explains:

- architectural principles

- engineering methodology

- observable behavior

- validation philosophy

- engineering evidence

The public documentation intentionally does not disclose:

- protected runtime implementation

- proprietary algorithms

- protected recovery logic

- protected authority mechanisms

- internal execution strategies

---

# Validation Status

The public validation program continues to evolve.

New engineering hypotheses are added only after they can be evaluated through reproducible execution.

The objective is continuous engineering validation rather than one-time certification.

---

# Intended Audience

This documentation is intended for:

- distributed systems engineers

- protocol architects

- runtime engineers

- systems researchers

- infrastructure engineers

- security engineers

- technical evaluators

- pilot participants

---

# Engineering Philosophy

Engineering should be evaluated through reproducible evidence.

Architectural discussion provides context.

Observable validation provides confidence.

Protected implementation preserves intellectual property.

These principles are intentionally independent.

---

# Closing Statement

The public specification describes what engineers can observe.

The protected runtime contains how those observable properties are achieved.

Both parts are necessary.

Only one must remain private.

**Continuity First.**