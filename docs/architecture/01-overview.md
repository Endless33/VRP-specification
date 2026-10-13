# VRP Architecture Overview

Status: Release Candidate (v1.0.0-rc1)

## Purpose

Veil Routing Protocol (VRP) is a continuity-first session protocol.

The protocol is designed so that an application's logical session can
survive transport changes, relay migration and network instability while
preserving a single canonical session identity.

VRP does not define a transport protocol.

Instead, it defines rules that allow transports to change without changing
the session itself.

## Design Goals

- Preserve session continuity.
- Separate session identity from transport identity.
- Make every canonical transition verifiable.
- Reject stale, replayed and unauthorized state transitions.
- Keep authority deterministic.

## Core Principles

### Session != Transport

A session is the canonical application identity.

Transport is only a temporary delivery mechanism.

Replacing transport must not create a new session.

### Continuity First

Network failures are expected.

The protocol is designed to recover whenever canonical authority permits.

### Authority Driven

Every state transition must be authorized.

Authority is the only source of canonical truth.

### Fail Closed

Ambiguous or invalid state transitions are rejected.

Canonical state must never advance on uncertain input.

### Verifiable Evidence

Protocol decisions should be reproducible and independently verifiable.

## Scope

This specification describes protocol behaviour.

It does not require a specific programming language,
operating system or transport implementation.

