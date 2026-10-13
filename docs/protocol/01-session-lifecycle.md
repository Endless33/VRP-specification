# Session Lifecycle

Status: Release Candidate (v1.0.0-rc1)

## Overview

A VRP session represents the canonical identity of an application communication context.

A session exists independently of the transport used to exchange protocol messages.

Transport changes do not create a new session.

## Lifecycle

A session progresses through a sequence of canonical states.

Created
↓
Initialized
↓
Transport Attached
↓
Active
↓
Migration
↓
Recovery
↓
Closed

Only one canonical state exists at any moment.

## Session Creation

A session is created by the protocol runtime.

The runtime assigns a canonical session identity.

## Active State

While active, a session may use one or more transports.

Transport replacement does not affect session identity.

## Migration

Migration replaces the transport while preserving the canonical session.

Migration must not create a second session.

## Recovery

Recovery restores communication after a temporary transport interruption.

Recovery continues the existing session.

## Session Termination

A closed session cannot be resumed.

Any future communication requires creation of a new canonical session.

## Canonical Guarantees

A compliant implementation shall guarantee:

- exactly one canonical session identity;
- exactly one canonical state;
- transport replacement without session replacement;
- deterministic state transitions;
- rejection of stale transitions;
- rejection of unauthorized transitions.

## Relation to Other Sections

This document describes only the session lifecycle.

Authority, replay protection, runtime behavior and evidence are specified in separate sections of this specification.
