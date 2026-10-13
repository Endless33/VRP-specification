# Authority Model

Status: Release Candidate (v1.0.0-rc1)

## Overview

Authority determines which state transitions are considered canonical.

No transition is valid unless it is authorized according to the protocol rules.

## Purpose

Authority prevents conflicting views of the same session.

It guarantees that only one canonical history exists.

## Canonical Authority

A compliant implementation shall ensure:

- exactly one canonical authority exists for a session;
- authority decisions are deterministic;
- authority cannot be duplicated;
- authority cannot be transferred implicitly.

## Authorized Transitions

Only authorized transitions may:

- create a session;
- activate a session;
- migrate a session;
- recover a session;
- terminate a session.

Any unauthorized transition shall be rejected.

## Authority Continuity

Transport migration does not change authority.

Relay migration does not change authority.

Network instability does not change authority.

Authority belongs to the canonical session rather than to any transport.

## Failure Handling

If authority cannot be established safely:

- the transition shall not be executed;
- canonical state shall remain unchanged;
- the implementation shall fail closed.

## Security Properties

The authority model is designed to provide:

- deterministic state ownership;
- protection against conflicting histories;
- replay resistance;
- canonical state consistency.

## Relation to Other Sections

Session lifecycle defines when transitions occur.

Authority defines whether those transitions are permitted.

Replay protection and evidence verification are defined separately.

