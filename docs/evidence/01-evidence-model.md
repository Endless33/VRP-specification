# VRP Evidence Model

Status: Release Candidate (v1.0.0-rc1)

## 1. Purpose

This document defines the role of evidence in validating observable
VRP behaviour and supporting protocol compliance claims.

Evidence records observations and verification results. It does not
replace protocol rules or canonical runtime state.

## 2. Evidence Objectives

Evidence should make it possible to assess:

- whether a protocol operation was attempted;
- whether the operation was accepted or rejected;
- whether canonical state changed;
- whether session identity was preserved;
- whether a reported outcome was independently verified.

## 3. Evidence Sources

Evidence may be produced by:

- protocol runtime observations;
- validation harnesses;
- adversarial test scenarios;
- independent verification tools;
- controlled pilot experiments.

Each source must be identified clearly enough to establish the
origin and scope of its claims.

## 4. Evidence Integrity

Evidence used to support a compliance claim must be protected
against unauthorized modification.

Where cryptographic digests, signatures or hash chains are used,
the verification procedure must define what is covered and which
trust anchors are required.

A digest alone does not establish that the original observation
was truthful.

## 5. Verification

A verifier should:

1. validate the evidence format;
2. validate required fields and trust anchors;
3. check integrity according to the applicable verification rules;
4. reject malformed or inconsistent evidence;
5. report the verification outcome explicitly.

A failed verification must not be reported as successful.

## 6. Determinism

Where deterministic evidence is claimed, the relevant inputs,
canonicalization rules and comparison criteria must be specified.

Equivalent inputs should produce equivalent verification outcomes
under the same verification rules.

Timestamps and other intentionally variable metadata must be
handled explicitly.

## 7. Negative Evidence

Evidence should record relevant rejected operations, including
replay, stale input, unauthorized transitions and invalid state
changes, where these cases are covered by the validation procedure.

A rejected request is not, by itself, proof that all related attack
classes are prevented.

## 8. Test Reports

A test report should identify:

- implementation and version;
- test scenario and configuration;
- inputs and relevant environmental conditions;
- expected result;
- observed result;
- PASS or FAIL verdict;
- known limitations.

Results must not be generalized beyond the tested conditions
without additional supporting evidence.

## 9. Independent Verification

Where independent verification is claimed, the verifier should
be distinguishable from the component that produced the evidence.

The report must identify the verification procedure and its
trust assumptions.

Running the same implementation twice does not, by itself,
constitute independent verification.

## 10. Security and Privacy

Evidence must not expose cryptographic secrets, private keys or
protected implementation details.

Identifiers and metadata should be limited to what is necessary
for the stated verification purpose.

## 11. Compliance Claims

Every public claim about continuity, replay resistance, integrity
or recovery must identify the relevant verification evidence.

A successful test demonstrates the tested behaviour under the
tested conditions. It does not prove universal correctness.

## 12. Related Documents

- Protocol: Canonical State Machine
- Security: VRP Security Model
- Runtime: Runtime Pipeline
- Runtime: Runtime Failover
- Runtime: Runtime Migration
- Runtime: Runtime Recovery

