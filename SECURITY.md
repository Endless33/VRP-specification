# Security Policy

## Purpose

This document describes the public security policy for the Veil Routing Protocol (VRP) specification.

The VRP project separates public engineering documentation from the protected runtime implementation.

This repository contains the public architectural specification.

It does not contain confidential implementation details.

---

# Security Philosophy

Security is achieved through architectural correctness.

The runtime assumes that:

- networks are unreliable;
- infrastructure may fail;
- communication paths may change;
- replay attempts may occur;
- concurrent execution may occur.

The objective is to preserve architectural invariants under these conditions.

---

# Scope

This repository includes:

- public specifications;
- RFC documents;
- ADR documents;
- engineering documentation;
- evaluation methodology;
- integration guidance.

It does not include:

- proprietary runtime implementation;
- internal algorithms;
- optimization strategies;
- protected source code.

---

# Reporting Security Issues

If you believe you have identified a security issue related to the public specification or evaluation process, please report it privately.

Please include:

- affected document;
- affected section;
- description of the issue;
- expected behavior;
- observed behavior;
- reproducible steps (if applicable);
- supporting evidence.

Reports with clear technical evidence are easier to evaluate.

---

# Responsible Disclosure

Please avoid publicly disclosing suspected security issues before they have been reviewed.

Responsible disclosure helps maintain accurate technical discussions and allows issues to be evaluated appropriately.

---

# Security Boundaries

The following areas are intentionally outside the scope of this public repository:

- Protected Runtime implementation;
- internal synchronization mechanisms;
- transport selection algorithms;
- authority coordination algorithms;
- proprietary optimization techniques;
- implementation heuristics.

These boundaries are documented throughout the public specification.

---

# Supported Security Topics

Appropriate public discussions include:

- architectural security;
- threat models;
- replay protection concepts;
- trust boundaries;
- evaluation methodology;
- observable runtime behavior.

Implementation-specific discussions may not be possible.

---

# Evaluation

Security claims should be supported by:

- observable runtime behavior;
- engineering evidence;
- reproducible evaluation;
- published specifications.

Evidence-based engineering discussion is encouraged.

---

# Security Principles

VRP is built upon the following security principles:

- Session ≠ Transport
- One Canonical Authority
- Replay Protection
- Deterministic Runtime Decisions
- Correctness Before Availability
- Protected Implementation
- Independent Validation

These principles guide architectural evolution.

---

# Acknowledgements

The project appreciates responsible security research and constructive engineering feedback.

Well-documented reports contribute to improving the public specification.

---

# Summary

The VRP public specification is designed to support transparent architectural evaluation while protecting proprietary implementation.

Security discussions should focus on observable engineering behavior and reproducible evidence.