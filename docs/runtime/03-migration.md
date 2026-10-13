# Runtime Migration

Status: Release Candidate (v1.0.0-rc1)

## Overview

Migration changes the transport path used by an existing VRP session
while preserving its canonical identity.

Migration is a protocol operation, not the creation of a replacement
session.

## Objectives

Migration is intended to allow a session to continue when its current
transport becomes unavailable, degraded or unsuitable.

The runtime must distinguish transport replacement from session
replacement.

## Preconditions

Before committing a migration, the implementation must establish that:

- the candidate path is available for the intended operation;
- the migration request is valid for the current session;
- the transition is authorized;
- the request is not stale or replayed;
- the transition is consistent with canonical session state.

The exact validation mechanism is implementation-specific unless
otherwise defined by the protocol specification.

## Migration Procedure

The runtime should perform the following logical steps:

1. Observe the need for migration.
2. Identify a candidate transport path.
3. Validate the candidate path.
4. Validate the migration request and its authority.
5. Commit the authorized canonical transition.
6. Confirm the resulting state.
7. Record verifiable evidence of the outcome.

These steps describe logical requirements, not a mandate for a
particular internal implementation or execution schedule.

## Identity and Authority

A successful transport migration SHALL preserve the canonical
session identity.

Migration SHALL NOT implicitly create a new session.

Migration SHALL NOT implicitly transfer canonical authority.

## Concurrent and Stale Requests

If multiple migration requests compete, the implementation must
ensure that only transitions valid under the canonical authority
and state rules can commit.

A stale, replayed or unauthorized request SHALL NOT mutate
canonical session state.

## Failure Handling

If migration cannot be validated or committed safely:

- the implementation SHALL reject the transition;
- canonical state SHALL NOT be partially updated;
- the implementation SHALL NOT report migration success without
  verifying the outcome.

The runtime may retain the existing session while communication
is unavailable, subject to its canonical lifecycle rules.

## Evidence

Migration evidence should make it possible to determine:

- which session was involved, using an appropriate identifier;
- what migration outcome was observed;
- whether authorization succeeded;
- whether canonical identity was preserved;
- whether the operation completed or was rejected.

Evidence must not expose secrets or protected internal state.

## Relation to Other Sections

Session Lifecycle defines the session states.

Authority Model defines authorization requirements.

Transport Independence defines the separation between session
identity and transport identity.

Replay Protection defines the handling of replayed messages.

Runtime Pipeline defines the logical processing stages.

