# VRP Compliance

Status: Release Candidate (v1.0.0-rc1)

## Purpose

This document defines the minimum observable requirements for an
implementation to claim compliance with the Veil Routing Protocol
(VRP) specification.

Compliance applies to externally observable protocol behaviour.

It does not require a specific programming language, operating
system or internal architecture.

## Minimum Requirements

A compliant implementation shall:

- preserve canonical session identity;
- separate session identity from transport identity;
- reject unauthorized state transitions;
- reject replayed protocol messages according to the applicable
  replay rules;
- execute canonical state transitions deterministically;
- preserve canonical state consistency;
- generate verifiable protocol evidence.

## Non-Requirements

This specification does not require:

- a specific transport protocol;
- a specific cryptographic library;
- a specific runtime implementation;
- a specific relay architecture;
- a specific operating system.

## Verification

A compliance claim should identify:

- implementation name;
- implementation version;
- specification version;
- executed validation procedures;
- verification evidence.

## Conformance Levels

An implementation may claim:

- Specification Conformance
- Runtime Conformance
- Security Conformance
- Evidence Conformance

Each claim shall identify the corresponding verification evidence.

## Limitations

Compliance with this specification does not imply the absence of
implementation defects or security vulnerabilities.

A compliance claim is limited to the behaviour demonstrated by the
available verification evidence.

