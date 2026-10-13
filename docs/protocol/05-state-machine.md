# Canonical State Machine

Status: Release Candidate (v1.0.0-rc1)

## Overview

VRP defines a single canonical finite state machine (FSM) for every
session.

At any moment exactly one canonical state exists.

State transitions are deterministic and require canonical authority.

## Canonical States

Created

Initialized

Transport Attached

Active

Migration

Recovery

Closed

## State Transitions

The following transitions are permitted:

Created
→ Initialized

Initialized
→ Transport Attached

Transport Attached
→ Active

Active
→ Migration

Migration
→ Active

Active
→ Recovery

Recovery
→ Active

Active
→ Closed

No other transition is considered canonical.

## Transition Rules

Every transition SHALL:

- be authorized;
- preserve canonical consistency;
- execute atomically;
- produce deterministic results.

## Invalid Transitions

A compliant implementation SHALL reject:

- replayed transitions;
- stale transitions;
- unauthorized transitions;
- conflicting transitions;
- ambiguous transitions.

Rejected transitions SHALL NOT mutate canonical state.

## Transport Independence

Transport replacement does not change the FSM.

Migration changes transport participation only.

Session identity and canonical state remain unchanged.

## Failure Handling

If a transition cannot be completed safely:

- canonical state remains unchanged;
- authority remains unchanged;
- the implementation fails closed.

## Compliance

Every compliant VRP implementation SHALL expose behaviour equivalent
to this canonical state machine regardless of programming language,
operating system or transport implementation.

