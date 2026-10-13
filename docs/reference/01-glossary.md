# VRP Glossary

Status: Release Candidate (v1.0.0-rc1)

## Purpose

This document defines the terminology used throughout the
Veil Routing Protocol (VRP) specification.

The definitions in this glossary are normative unless otherwise
specified.

## Session

The canonical identity of an application communication context.

A session exists independently of any individual transport.

## Transport

A mechanism used to exchange protocol messages between endpoints.

Transport does not define session identity.

## Canonical State

The single protocol state considered authoritative for a session.

## Canonical Transition

A state transition accepted according to the protocol's
authorization and validation rules.

## Authority

The protocol mechanism responsible for determining whether a
requested state transition is valid.

## Migration

Replacement of the transport used by an existing session while
preserving canonical session identity.

## Recovery

Restoration of communication for an existing canonical session
after interruption.

## Failover

Selection of an alternative communication path after loss or
degradation of the current path.

## Replay

A protocol message that has already been processed or is otherwise
no longer valid for acceptance.

## Evidence

Observable information produced during protocol execution or
validation and used to support verification.

## Verification

The process of evaluating evidence against the applicable protocol
requirements.

## Continuity

Preservation of canonical session identity throughout valid runtime
operations.

## Compliance

Conformance of an implementation with the observable requirements
defined by this specification.

