# Replay Protection

Status: Release Candidate (v1.0.0-rc1)

## Overview

Replay protection prevents previously accepted protocol messages from
being accepted again.

A replayed message shall never modify canonical session state.

## Design Objective

The protocol assumes that networks may duplicate, delay or reorder
traffic.

Such behaviour shall not affect the canonical session.

## Canonical Rule

A protocol message SHALL be accepted at most once.

Messages that have already contributed to canonical state SHALL NOT
be accepted again.

## Threat Model

Replay protection addresses:

- duplicated packets;
- delayed packets;
- reordered packets;
- captured traffic replay;
- stale protocol messages.

## Canonical Guarantees

Replay protection SHALL preserve:

- canonical session identity;
- canonical authority;
- canonical state consistency;
- deterministic protocol behaviour.

## Failure Handling

If a message cannot be proven to be valid for the current canonical
state, it SHALL be rejected.

Rejected messages SHALL NOT mutate protocol state.

The implementation SHALL fail closed.

## Security Properties

Replay protection provides:

- deterministic acceptance;
- resistance to duplicated traffic;
- resistance to delayed delivery;
- protection against stale state transitions.

## Relation to Other Sections

Session Lifecycle defines valid session evolution.

Authority determines whether a transition is permitted.

Replay Protection ensures that obsolete protocol messages cannot
re-enter the canonical history.

