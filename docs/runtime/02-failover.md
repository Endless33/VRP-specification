# Runtime Failover

Status: Release Candidate (v1.0.0-rc1)

## Overview

Failover is the process of moving session traffic away from an
unavailable or unsuitable path while preserving canonical session
identity.

Failover changes the delivery path. It does not, by itself, create
a new session or transfer canonical authority.

## Failure Detection

A runtime may detect path failure through transport errors,
health observations, timeouts or other implementation-specific
signals.

Failure detection is an observation, not an authorization to
change canonical session state.

## Failover Requirements

A compliant implementation shall:

- preserve the existing session identity;
- validate the candidate path before using it;
- preserve canonical authority;
- reject stale or unauthorized transitions;
- avoid committing partial state changes;
- produce evidence for the resulting decision.

## Candidate Path Selection

The runtime may evaluate alternative paths using implementation-
specific metrics and policy.

A path being available does not automatically make it valid.

The candidate path must satisfy the applicable protocol and
authorization requirements.

## Failure Handling

If no acceptable path is available, the runtime shall not fabricate
successful recovery.

The session may remain unable to exchange traffic until an acceptable
path becomes available or a valid termination transition occurs.

## Authority Continuity

Transport or relay failover does not implicitly transfer authority.

Any authority change must follow the protocol's canonical
authorization rules.

## Evidence

Implementations should record enough information to verify:

- the observed failure;
- the selected action;
- the authorization decision;
- the resulting canonical state;
- whether continuity was preserved.

Evidence must not be represented as proof of successful continuity
unless the relevant outcome was actually verified.

## Relation to Other Sections

Session Lifecycle defines session states.

Authority Model defines transition authorization.

Transport Independence defines the separation between session
identity and transport.

Runtime Pipeline defines the execution stages.

