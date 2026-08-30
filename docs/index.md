# Veil Routing Protocol (VRP)

## Public Engineering Specification

Welcome to the public engineering specification of the **Veil Routing Protocol (VRP)**.

VRP is a continuity-first runtime architecture designed for distributed systems operating across changing communication infrastructure.

Rather than binding execution to a transport, VRP introduces a protected runtime responsible for preserving deterministic execution through a stable Logical Session.

---

# Guiding Principle

> **Logical Session ≠ Transport**

A transport may change.

A Logical Session should remain valid whenever architectural correctness permits.

This principle serves as the foundation of the VRP architecture.

---

# Documentation Structure

## Architecture

Describes the fundamental concepts of VRP.

Includes:

- Session ≠ Transport
- Authority Model
- Runtime State Machine
- Architectural Invariants

---

## RFC Series

The RFC collection defines the public architectural contracts of VRP.

RFCs specify observable behavior.

They do not describe implementation.

---

## ADR Series

Architecture Decision Records explain why major engineering decisions were made.

Each ADR documents architectural intent rather than implementation.

---

## Runtime

Describes:

- Runtime lifecycle
- Authority transitions
- Event flow
- Recovery rules
- Failure handling
- Runtime invariants

---

## Security

Documents the public security architecture including:

- Threat Model
- Trust Boundaries
- Security Model
- Replay Protection
- Protected Runtime Boundary

---

## Evaluation

Defines the engineering validation methodology.

Includes:

- Test Matrix
- PASS Criteria
- FAILURE Criteria
- Reproducibility
- Evidence Format
- Audit Guide

Engineering conclusions are based upon observable runtime behavior.

---

## Integration

Explains how applications integrate with the Protected Runtime.

Topics include:

- Runtime API
- Embedding
- Runtime Events
- Callbacks
- Configuration
- Transport Abstraction

---

## Engineering

Engineering documentation includes:

- Design Principles
- Roadmap
- Versioning Policy
- Changelog

These documents describe long-term architectural evolution.

---

## Whitepaper

The Whitepaper collection provides:

- Executive Summary
- Engineering Overview
- Complete Whitepaper

These documents summarize the public specification from different engineering perspectives.

---

## Examples

Conceptual integration examples include:

- Minimal Integration
- Server Applications
- Client Applications
- IoT Systems
- Edge Computing

Examples describe architecture rather than implementation.

---

# Engineering Philosophy

The VRP public specification intentionally separates:

**Observable Architecture**

from

**Protected Implementation**

The specification defines:

- architectural guarantees;
- observable behavior;
- engineering contracts;
- validation methodology.

Implementation details remain confidential.

---

# Independent Validation

The architecture is designed to support independent engineering evaluation.

Observable runtime behavior may be validated through:

- published specifications;
- engineering evidence;
- reproducible evaluation;
- independent audit.

Implementation disclosure is unnecessary.

---

# Intended Audience

This specification is intended for:

- software architects
- distributed systems engineers
- networking engineers
- security engineers
- technical evaluators
- engineering leadership
- technology partners

---

# Repository Overview

This repository contains:

- RFC Series
- ADR Series
- Architecture Documentation
- Runtime Documentation
- Security Documentation
- Evaluation Documentation
- Integration Documentation
- Engineering Documentation
- Whitepapers
- Engineering Examples

Together these documents define the public engineering architecture of the Veil Routing Protocol.

---

# License

Refer to the repository LICENSE file for licensing information.

---

# Learn More

Start with:

1. Executive Summary
2. Engineering Overview
3. Whitepaper
4. Architecture
5. RFC Series
6. Runtime Documentation
7. Evaluation Documentation
8. Integration Documentation

This reading order provides a gradual introduction from high-level concepts to detailed engineering specifications.