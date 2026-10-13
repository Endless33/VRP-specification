# Transport Independence

Status: Release Candidate (v1.0.0-rc1)

## Overview

VRP separates session identity from transport identity.

A transport provides connectivity.

A session provides canonical application continuity.

The protocol does not bind a session to a single transport.

## Design Objective

Transport failures are expected.

Transport replacement is considered a normal protocol operation.

The objective of VRP is to preserve the session while transports may
appear, disappear or change.

## Canonical Rule

Session identity SHALL remain unchanged when the transport changes.

Changing transport SHALL NOT create a new session.

Changing transport SHALL NOT invalidate canonical authority.

## Supported Operations

A compliant implementation may:

- replace one transport with another;
- migrate between available paths;
- recover after transport interruption;
- continue an existing session after transport restoration.

## Unsupported Assumptions

The protocol does not assume:

- permanent TCP connections;
- stable IP addresses;
- fixed relay paths;
- uninterrupted network connectivity.

## Canonical Guarantees

Transport replacement SHALL preserve:

- session identity;
- authority;
- canonical state;
- protocol continuity.

## Failure Handling

Loss of transport alone does not terminate a session.

Session termination requires an explicit canonical transition.

## Engineering Consequences

Implementations are free to support different transport technologies.

Protocol correctness depends on canonical state rather than transport
implementation.

## Relation to Other Sections

Session Lifecycle defines when migration occurs.

Authority defines who authorizes migration.

Replay Protection defines how obsolete traffic is rejected.

