# Getting Started

## Status

Public Integration Guide

Version: 2.0

---

# Introduction

This document explains how engineering teams begin evaluating the observable behavior of the VRP Runtime.

The objective is to make evaluation straightforward while preserving the protected implementation boundary.

This guide describes the public evaluation workflow only.

---

# Intended Audience

This documentation is intended for:

- Backend Engineers
- Platform Engineers
- Systems Engineers
- Network Engineers
- Infrastructure Teams
- Site Reliability Engineers (SRE)
- Security Engineers
- Enterprise Architecture Teams
- Technical Evaluators

Previous knowledge of VRP internals is not required.

---

# Evaluation Philosophy

The purpose of an evaluation is not to inspect protected implementation.

The purpose is to determine whether the observable runtime behavior satisfies engineering requirements.

Evaluation focuses on reproducible runtime behavior.

---

# Before You Begin

Before starting an evaluation, reviewers should understand the following principles.

- Session ≠ Transport
- Runtime decisions are deterministic
- Authority is canonical
- Replay is rejected
- Stale authority is rejected
- Observable evidence is reproducible
- Protected implementation remains private

These principles define the evaluation boundary.

---

# Evaluation Process

The recommended workflow is shown below.

```
Read Documentation
        │
        ▼
Understand Architecture
        │
        ▼
Review Security Boundary
        │
        ▼
Review Runtime Model
        │
        ▼
Prepare Evaluation Environment
        │
        ▼
Execute Validation
        │
        ▼
Review Evidence
        │
        ▼
Engineering Decision
```

---

# Public Evaluation Scope

The public evaluation allows engineers to observe:

- runtime behavior
- authority transitions
- recovery behavior
- replay rejection
- duplicate execution handling
- transport evolution
- evidence generation
- deterministic runtime decisions

The public evaluation does not expose proprietary implementation.

---

# What Is Not Required

Engineering evaluation does not require access to:

- runtime source code
- internal algorithms
- cryptographic material
- protocol encoding
- scheduler implementation
- optimization logic
- proprietary runtime architecture

Evaluation focuses on observable behavior.

---

# Engineering Questions

During evaluation, teams are encouraged to investigate questions such as:

- Does logical continuity behave as expected?
- Is authority deterministic?
- Are replay attempts rejected?
- Is recovery reproducible?
- Does observable behavior remain consistent?
- Can evidence be independently reviewed?

These questions are more important than implementation details.

---

# Recommended Evaluation Order

The recommended document order is:

1. SESSION_NOT_TRANSPORT.md
2. RUNTIME_MODEL.md
3. AUTHORITY_MODEL.md
4. FAILURE_MODEL.md
5. STATE_MACHINE.md
6. EVENT_FLOW.md
7. VRP_PROTOCOL_OVERVIEW.md
8. SECURITY_BOUNDARY.md

Following this sequence provides the best understanding of the architecture.

---

# Engineering Mindset

VRP should be evaluated as a runtime architecture.

It should not be evaluated as:

- a traditional VPN
- a transport protocol replacement
- a routing protocol
- a tunneling library
- a networking utility

Its primary objective is deterministic continuity.

---

# Success Criteria

A successful evaluation should answer:

- Is runtime behavior deterministic?
- Are architectural invariants preserved?
- Can evidence be reproduced?
- Is authority evolution understandable?
- Are runtime boundaries clearly defined?

These questions form the basis of engineering confidence.

---

# Protected Boundary

The following remain outside public evaluation:

- runtime implementation
- protected algorithms
- protocol internals
- source code
- deployment mechanisms
- proprietary architecture

These components are intentionally excluded.

---

# Next Steps

After completing the public documentation review, engineering teams may continue with:

- Pilot documentation
- Evaluation procedures
- Integration planning
- Runtime validation
- Evidence review

Further access, if applicable, is determined individually.

---

# Summary

The public evaluation is designed to answer one question:

Can observable runtime behavior be independently trusted without requiring disclosure of protected implementation?

The answer should come from reproducible engineering evidence.

---

> Evaluate observable behavior.

> Validate engineering claims.

> Trust reproducible evidence.