# Runtime Pipeline

Status: Release Candidate (v1.0.0-rc1)

## Overview

The VRP runtime executes protocol decisions while preserving the
canonical session state.

The runtime is responsible for processing protocol events in a
deterministic order.

## Objectives

The runtime shall:

- preserve session continuity;
- execute canonical state transitions;
- reject invalid protocol events;
- produce deterministic results;
- preserve protocol evidence.

## Processing Pipeline

A protocol event passes through the following stages:

1. Receive
2. Validate
3. Authorize
4. Execute
5. Commit
6. Produce Evidence

Every stage completes before the next stage begins.

## Determinism

Given identical canonical inputs, every compliant implementation
shall produce identical runtime behaviour.

## Failure Handling

If any pipeline stage fails:

- canonical state remains unchanged;
- no partial transition is committed;
- the runtime fails closed.

## Runtime Independence

The runtime model is independent of:

- operating system;
- transport implementation;
- programming language.

Only observable protocol behaviour is defined by this specification.

## Relation to Other Sections

The runtime executes the protocol defined in the Protocol section.

Security and evidence generation are described separately.

