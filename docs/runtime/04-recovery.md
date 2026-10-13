# Runtime Recovery

Status: Release Candidate (v1.0.0-rc1)

## Overview

Recovery restores communication for an existing VRP session after
a transport interruption or temporary loss of connectivity.

Recovery must preserve canonical session identity whenever the
session remains valid under the protocol lifecycle rules.

## Objectives

Recovery is intended to restore communication without creating
a replacement session merely because connectivity was interrupted.

The runtime must distinguish recovery of an existing session from
creation of a new session.

## Preconditions

Before committing a recovery transition, the implementation must
establish that:

- the session remains valid under canonical lifecycle rules;
- the recovery request is valid for the current session;
- the transition is authorized;
- stale and replayed requests are rejected;
- the recovered transport or path is suitable for use.

## Recovery Procedure

The runtime should perform the following logical steps:

1. Observe that communication may be restored.
2. Identify an available transport or path.
3. Validate the candidate path.
4. Validate session identity and current canonical state.
5. Validate authority and replay constraints.
6. Commit the authorized recovery transition.
7. Verify the resulting state.
8. Record evidence of the outcome.

These steps specify logical requirements, not a mandatory
internal execution schedule.

## Identity and Authority

Successful recovery of an existing session SHALL preserve
its canonical session identity.

Recovery SHALL NOT implicitly create a replacement session.

Recovery SHALL NOT implicitly transfer canonical authority.

## Stale and Conflicting Requests

A recovery request that is stale, replayed, unauthorized or
inconsistent with canonical state SHALL be rejected.

Rejected requests SHALL NOT mutate canonical session state.

Concurrent requests must be resolved according to the canonical
authority and state-transition rules.

## Failure Handling

If recovery cannot be validated or committed safely:

- the transition SHALL be rejected;
- canonical state SHALL NOT be partially updated;
- the implementation SHALL NOT claim successful recovery without
  verifying the outcome.

If the session is no longer valid under its lifecycle rules,
recovery must not silently reopen it.

## Evidence

Recovery evidence should make it possible to determine:

- whether recovery was attempted;
- whether the request was authorized;
- whether session identity was preserved;
- whether communication was actually restored;
- whether the transition completed or was rejected.

Evidence must not expose secrets or protected internal state.

## Relation to Other Sections

Session Lifecycle defines valid session states.

Authority Model defines authorization requirements.

Transport Independence defines the relationship between session
identity and transport identity.

Replay Protection defines how obsolete messages are handled.

Runtime Pipeline defines the logical processing stages.

Runtime Failover and Runtime Migration describe related path
selection and transport replacement behaviour.

