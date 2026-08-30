# ADR-0004 — Evidence-First Validation

**Status:** Accepted

**ADR Number:** 0004

**Date:** 2026

**Category:** Engineering Validation

---

# Context

Engineering discussions frequently begin with implementation.

Questions often focus on:

- source code
- algorithms
- implementation details
- optimization techniques

While implementation is important, implementation alone does not demonstrate observable correctness.

The VRP architecture instead places engineering evidence at the center of technical evaluation.

---

# Problem

Traditional technology evaluations frequently depend upon:

- architecture presentations
- implementation walkthroughs
- feature lists
- benchmark summaries
- vendor demonstrations

These approaches may not provide reproducible evidence of runtime behavior.

Engineering teams require observable facts rather than implementation claims.

---

# Decision

VRP adopts an **Evidence-First Validation** model.

Every significant engineering conclusion should be supported by observable evidence.

Engineering evidence becomes the primary basis for technical evaluation.

Implementation details remain secondary.

---

# Decision Drivers

This decision was motivated by the following objectives.

- reproducibility
- independent validation
- deterministic engineering
- observable correctness
- implementation independence
- objective technical review

Evidence should be reviewable by third parties.

---

# Alternatives Considered

## Alternative A

Implementation-first evaluation.

Advantages:

- implementation transparency

Disadvantages:

- difficult independent validation
- implementation bias
- unnecessary disclosure

Rejected.

---

## Alternative B

Presentation-driven evaluation.

Advantages:

- simple communication

Disadvantages:

- subjective conclusions
- difficult reproducibility
- insufficient engineering confidence

Rejected.

---

## Alternative C

Evidence-first evaluation.

Advantages:

- reproducible
- independently reviewable
- implementation independent
- engineering focused
- objective

Accepted.

---

# Consequences

Engineering discussions become centered around:

- observable runtime behavior
- validation scenarios
- engineering evidence
- deterministic outcomes
- reproducible reports

Implementation remains protected.

---

# Benefits

Evidence-first validation provides:

- objective engineering review
- repeatable technical assessment
- higher engineering confidence
- stronger auditability
- reduced implementation dependency

---

# Trade-offs

Producing high-quality evidence requires additional engineering effort.

Validation infrastructure, reporting and reproducibility become first-class architectural components.

This increases development effort but significantly improves evaluation quality.

---

# Architectural Impact

This decision directly supports:

RFC-0005 — Evidence Model

RFC-0007 — Failure Recovery

RFC-0009 — Security Boundary

RFC-0011 — Pilot Integration

Engineering evidence becomes a permanent architectural artifact rather than an optional report.

---

# Validation

An engineering conclusion should ideally answer:

- Which observable behavior was evaluated?
- Which invariant was validated?
- Which evidence supports the conclusion?
- Can another engineering team reproduce the result?

If these questions cannot be answered, the conclusion should be considered incomplete.

---

# Status

Accepted.

Evidence-first validation is considered a permanent engineering principle of the VRP architecture.

---

# Design Statement

Implementation explains how a runtime works.

Evidence demonstrates that it works.

Engineering decisions should be based on observable facts rather than assumptions.