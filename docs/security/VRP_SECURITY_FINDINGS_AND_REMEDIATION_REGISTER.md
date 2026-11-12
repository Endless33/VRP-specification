
# VRP SECURITY FINDINGS & RED-TO-GREEN REMEDIATION REGISTER

## Veil Routing Protocol (VRP)
### Security Defect Accountability, Corrective Engineering and Regression Evidence

**Document ID:** VRP-SFRR-005  
**Version:** 1.0  
**Date:** 2026-11-12  
**Status:** Public Engineering Review Baseline  
**Classification:** PUBLIC — NON-PROPRIETARY  
**Author:** Vitalijus Riabovas  
**Project:** Veil Routing Protocol / Jumping VPN

---

# 1. Executive Summary

The Veil Routing Protocol (VRP) is an experimental continuity-first networking architecture based on the principle:

**SESSION != TRANSPORT**

The architecture separates logical-session continuity from the lifetime of individual network transports.

This separation creates security-sensitive engineering obligations.

Transport replacement must not introduce unauthorized authority.

Replay rejection must not permit duplicate execution.

Recovery must not silently corrupt canonical state.

Rejected operations must not create unauthorized persistent effects.

These properties cannot be established through architectural declarations alone.

They require adversarial testing, reproducible failure analysis, corrective engineering and regression evidence.

This document defines the public security finding and remediation accountability framework for VRP.

It records selected historical engineering checkpoints and establishes the evidence requirements for publishing verified security corrections.

The central engineering principle is:

**A SECURITY FIX IS NOT COMPLETE UNTIL THE ORIGINAL FAILURE IS REPRODUCED, THE CORRECTION IS VERIFIED, AND THE RELEVANT REGRESSION CONTRACT REMAINS SATISFIED.**

This document does not claim that all VRP vulnerabilities have been discovered or eliminated.

---

# 2. Purpose

The remediation register provides a structured relationship between:

1. Security findings.
2. Violated protocol invariants.
3. Original failure evidence.
4. Affected implementation revisions.
5. Corrective changes.
6. Post-correction verification.
7. Regression testing.
8. Residual risks.
9. Finding closure decisions.

The register is designed to support:

- Internal security engineering.
- Independent protocol review.
- Enterprise due diligence.
- Controlled pilot qualification.
- Release assurance.
- Long-term security maintenance.

The public register intentionally excludes proprietary implementation mechanisms.

---

# 3. Security Engineering Principles

VRP remediation follows several non-negotiable principles.

## 3.1 Evidence before closure

A finding must not be marked verified merely because a patch exists.

## 3.2 Preserve the original contract

A failing security test must not be made green by weakening the invariant it was intended to enforce.

## 3.3 Attribute every correction

A correction must be associated with an identifiable source revision.

## 3.4 Preserve failure evidence

The original RED result must remain attributable after remediation.

## 3.5 Verify positive behavior

A security correction must not silently destroy legitimate functionality.

## 3.6 Separate model and runtime results

A passing public reference model does not establish that a private runtime has been corrected.

## 3.7 Protect proprietary implementation

Public disclosure should describe the security obligation, observed failure class, correction scope and verification outcome without exposing Protected Core algorithms.

---

# 4. RED-to-GREEN Verification Lifecycle

The standard remediation lifecycle is:

    INSPECT
       |
       v
    DEFINE SECURITY CONTRACT
       |
       v
    CONSTRUCT ADVERSARIAL REGRESSION
       |
       v
    RED — REPRODUCE VIOLATION
       |
       v
    ATTRIBUTE FAILURE
       |
       v
    APPLY SCOPED CORRECTION
       |
       v
    GREEN — VERIFY ORIGINAL SCENARIO
       |
       v
    RUN RELATED REGRESSIONS
       |
       v
    VERIFY SOURCE AND EVIDENCE
       |
       v
    RECORD RESIDUAL RISK
       |
       v
    CLOSE OR RETAIN FINDING

Each stage must produce sufficient evidence for its declared purpose.

A skipped stage must be disclosed.

---

# 5. Finding Status Vocabulary

Every finding must use an explicit lifecycle status.

| Status | Meaning |
|---|---|
| OPEN | Finding identified; correction not verified |
| UNDER INVESTIGATION | Failure analysis ongoing |
| RED CONFIRMED | Original security violation reproduced |
| CORRECTION IMPLEMENTED | Candidate correction exists |
| GREEN VERIFIED | Original scenario passes against corrected revision |
| REGRESSION VERIFIED | Relevant regression tests pass |
| CLOSED — VERIFIED | Required closure evidence accepted |
| CLOSED — ACCEPTED RISK | Residual risk formally accepted by an authorized owner |
| BLOCKED | Verification prevented by an identified prerequisite |
| EVIDENCE PENDING | Required public evidence not yet published |
| REOPENED | Previously closed finding requires renewed investigation |

A correction commit alone is insufficient to assign CLOSED — VERIFIED.

---

# 6. Finding Severity Model

Severity must be based on demonstrated impact and documented attack preconditions.

## Critical

A demonstrated violation permits severe unauthorized execution, authority compromise or canonical-state corruption under applicable threat assumptions.

## High

A demonstrated violation materially weakens a major security boundary.

## Medium

A demonstrated weakness affects a bounded security property and requires correction.

## Low

A demonstrated issue has limited security impact under the evaluated assumptions.

## Informational

An engineering observation or assurance limitation without a demonstrated exploitable security violation.

Severity must not be assigned solely from the name of a test or security component.

Where sufficient impact evidence is unavailable, severity remains:

**NOT YET ASSESSED**

---

# 7. Canonical Finding Record Schema

Every formal security finding should contain:

    FINDING_ID
    TITLE
    DISCOVERY_DATE
    AFFECTED_INVARIANT
    THREAT_CATEGORY
    AFFECTED_RELEASE
    ORIGINAL_SOURCE_REVISION
    FAILURE_DESCRIPTION
    RED_TEST_ID
    RED_EVIDENCE_REFERENCE
    ROOT_CAUSE_CLASSIFICATION
    CORRECTION_REVISION
    CORRECTION_SCOPE
    GREEN_TEST_ID
    GREEN_EVIDENCE_REFERENCE
    REGRESSION_EVIDENCE
    POSITIVE_CONTROL
    RESIDUAL_RISK
    SEVERITY
    STATUS
    REVIEWER
    CLOSURE_DATE

Fields that cannot be supported by attributable evidence must be marked:

    NOT ESTABLISHED

or:

    EVIDENCE PENDING

They must not be filled with assumed values.

---

# 8. Historical Engineering Checkpoint Register

The following records identify selected VRP engineering workstreams.

These are historical engineering references, not a claim that all associated remediation evidence is publicly available.

| Record ID | Engineering area | Known checkpoint | Public assurance status |
|---|---|---|---|
| VRP-FND-001 | Rejected endpoint adoption | 4cfc77ab | Evidence attribution pending |
| VRP-FND-002 | Replay admission linearizability | d7806b4b | Evidence attribution pending |
| VRP-FND-003 | Concurrent recovery state consistency | f2c697c0 | Evidence attribution pending |
| VRP-FND-004 | Canonical aggregation integrity | 702addcb | Evidence attribution pending |
| VRP-FND-005 | Release authority boundary | Stage 60 verification record | Public evidence pending |
| VRP-FND-006 | Unknown traffic and autonomous rejection | Planned adversarial qualification | Not yet established |

Checkpoint identifiers are repository-specific.

They must not be interpreted as globally unique without the corresponding repository identity.

The public release must verify that each referenced commit exists in its stated source repository before publication.

---

# 9. VRP-FND-001 — Rejected Endpoint Adoption

## 9.1 Security concern

A network endpoint must not become authorized solely because traffic was received from it.

A rejected packet must not create persistent path authority.

The relevant security principle is:

**REJECTED INPUT != AUTHORIZED PATH ADOPTION**

## 9.2 Associated invariant

VRP-INV-011 — Endpoint Adoption Safety.

## 9.3 Threat category

- Endpoint manipulation.
- Unauthorized path adoption.
- Transport authority confusion.

## 9.4 Historical engineering checkpoint

**Repository:** vrp-stage9s-validation

**Branch:** fix/stage9s-integrity-hardening

**Commit:**

    4cfc77abf81a700b379ead63f0fa14814cdbb419

**Recorded change description:**

    wireudp: prevent rejected endpoint path adoption

This checkpoint identifies a scoped engineering correction related to rejected endpoint adoption.

## 9.5 Required RED evidence

A qualifying original failure record must demonstrate:

1. An unauthorized or rejected packet was submitted.
2. The operation was rejected or otherwise failed admission.
3. A protected endpoint/path state changed contrary to the intended contract.

The original failing revision and execution log must be attributable.

## 9.6 Required GREEN evidence

The corrected revision must demonstrate:

    REJECTED_INPUT = TRUE

    UNAUTHORIZED_PATH_ADOPTION = FALSE

    CANONICAL_STATE_INTEGRITY = PRESERVED

## 9.7 Positive control

A legitimately authorized endpoint transition must remain possible under the applicable protocol contract.

## 9.8 Public closure status

**EVIDENCE PENDING**

The historical correction checkpoint is identified.

A complete public RED-to-GREEN evidence bundle is required before independent closure can be claimed.

---

# 10. VRP-FND-002 — Replay Admission Linearizability

## 10.1 Security concern

Concurrent admission must not permit multiple successful admissions of the same protected operation when the protocol contract permits only one.

Sequential replay rejection is not sufficient evidence of concurrency safety.

## 10.2 Associated invariants

VRP-INV-003 — Replay Containment.

VRP-INV-012 — Concurrent Admission Safety.

## 10.3 Threat category

- Concurrent duplicate admission.
- Replay races.
- Duplicate canonical execution.

## 10.4 Historical engineering checkpoint

**Repository:** jumping-vpn-core

**Branch:** stage11-bounded-runtime

**Commit:**

    d7806b4b14998a97c25b5b86e353c6870da844ca

**Recorded change description:**

    security: replay linearizability hardening
    (window and runtime)

The recorded correction scope was limited to two replay-related production files.

The implementation details are not disclosed in this public register.

## 10.5 Required RED evidence

The original failure must demonstrate a violation of the applicable concurrent admission contract.

The evidence should include:

- Original source revision.
- Concurrent operation count.
- Duplicate admission count.
- Canonical-state observations.
- Failure reproduction command.
- Execution log.

## 10.6 Required GREEN evidence

The corrected revision must demonstrate that concurrent duplicate submissions cannot violate the applicable admission contract.

For a previously accepted protected operation:

    ADDITIONAL_DUPLICATE_ACCEPTANCES = 0

## 10.7 Regression requirements

- Sequential replay tests.
- Concurrent duplicate tests.
- Repeated scheduling.
- Race detection.
- Legitimate sequence progression.
- Canonical-state consistency.

Race-detector success alone does not establish linearizability.

## 10.8 Public closure status

**EVIDENCE PENDING**

The correction checkpoint is identified.

A public, revision-qualified RED-to-GREEN chain is required for independently reviewable closure.

---

# 11. VRP-FND-003 — Concurrent Recovery State Consistency

## 11.1 Security concern

Concurrent mutation and observation of recovery state may produce inconsistent reports or unsafe assumptions.

The security-relevant objective is to ensure that recovery observations correspond to a coherent state.

## 11.2 Associated invariants

VRP-INV-007 — Canonical History Integrity.

VRP-INV-009 — Recovery Integrity.

VRP-INV-012 — Concurrent Admission Safety, where applicable.

## 11.3 Historical engineering checkpoint

**Repository:** vrp-stage9s-validation

**Branch:** fix/stage9s-integrity-hardening

**Commit prefix:**

    f2c697c0

**Recorded change description:**

    stage9s: make run report snapshot-coherent

The historical engineering work addressed consistency of recovery reporting under concurrent mutation.

## 11.4 Required RED evidence

A qualifying failure record should demonstrate an inconsistent or unsafe recovery observation under concurrency.

## 11.5 Required GREEN evidence

The corrected implementation should demonstrate coherent recovery reporting under the declared concurrency conditions.

## 11.6 Regression requirements

- Concurrent state mutation.
- Snapshot consistency.
- Race detection.
- Repeatability.
- Existing recovery behavior.

## 11.7 Public closure status

**EVIDENCE PENDING**

The correction checkpoint is recorded.

Public closure requires the complete source identity and corresponding evidence bundle.

---

# 12. VRP-FND-004 — Canonical Aggregation Integrity

## 12.1 Security concern

Aggregated recovery state must not claim capabilities or outcomes unsupported by actual execution.

An inaccurate recovery report may cause downstream systems or reviewers to accept an invalid operational conclusion.

## 12.2 Associated invariants

VRP-INV-007 — Canonical History Integrity.

VRP-INV-009 — Recovery Integrity.

VRP-INV-013 — Evidence Integrity.

## 12.3 Historical engineering checkpoint

**Repository:** vrp-stage9s-validation

**Branch:** fix/stage9s-integrity-hardening

**Commit prefix:**

    702addcb

**Recorded change description:**

    stage9s: remove unused proof capability
    from final state aggregator

## 12.4 Security objective

Reported recovery capabilities must correspond to actual supported behavior.

A reported operation count must not be interpreted as proof that an operation occurred unless supported by execution evidence.

## 12.5 Required verification

- Aggregated-state consistency.
- Capability-report accuracy.
- Regression tests.
- Source attribution.
- Evidence integrity.

## 12.6 Public closure status

**EVIDENCE PENDING**

The checkpoint establishes a historical engineering change.

Its public security assurance classification requires supporting evidence.

---

# 13. VRP-FND-005 — Release Authority Boundary

## 13.1 Security concern

Release authenticity and authorization dependencies must be enforced before protected admission decisions.

A missing or invalid authentication dependency must not silently result in successful admission.

## 13.2 Associated invariants

VRP-INV-005 — Stale Authority Rejection.

VRP-INV-010 — Fail-Closed Admission.

VRP-INV-013 — Evidence Integrity.

## 13.3 Historical engineering reference

**Project:** VRP Sovereign Guardian

**Engineering stage:** Stage 60

**Recorded result:**

    STAGE_60=VERIFIED_COMBINED_GREEN

The historical Stage 60 evaluation recorded:

- Targeted admission tests passing.
- Repeated regression execution.
- Full repository regression passing.
- Go vet passing.
- Invalid signature rejection.
- Unknown key rejection.
- Replay-after-denial rejection.
- Missing authenticator rejection.

These are internal engineering observations.

They must not be represented as independently reproduced public evidence without publication of the corresponding artifacts.

## 13.4 Required security behavior

    INVALID_RELEASE_AUTHORITY = REJECTED

    UNKNOWN_SIGNING_KEY = REJECTED

    MISSING_AUTHENTICATOR = REJECTED

    UNAUTHORIZED_ADMISSION = FALSE

## 13.5 Required evidence

- Source revision.
- Test configuration.
- Test execution log.
- Negative control.
- Positive control.
- Regression result.
- Evidence integrity verification.

## 13.6 Public closure status

**INTERNAL VERIFICATION RECORDED — PUBLIC EVIDENCE PENDING**

This classification preserves the distinction between completed internal testing and independent public assurance.

---

# 14. VRP-FND-006 — Unknown Traffic and Autonomous Rejection

## 14.1 Security concern

Unknown or malformed network input must not acquire authority through ambiguous admission behavior.

The protocol must handle unexpected input without unauthorized persistent-state mutation.

## 14.2 Associated invariants

VRP-INV-010 — Fail-Closed Admission.

VRP-INV-014 — Bounded Resource Behavior.

## 14.3 Threat categories

- Malformed packet injection.
- Unknown message types.
- Unexpected input sequences.
- Sustained invalid traffic.
- Recovery under adversarial pressure.

## 14.4 Required behavior

    UNKNOWN_INPUT != TRUSTED_INPUT

    INVALID_ADMISSION != AUTHORIZED_EXECUTION

    REJECTED_OPERATION != CANONICAL_COMMIT

## 14.5 Verification requirements

- Isolated adversarial environment.
- Recorded source revision.
- Reproducible malformed-input corpus.
- Admission decision observations.
- Canonical-state integrity checks.
- Resource measurements.
- Legitimate positive controls.

## 14.6 Status

**QUALIFICATION NOT YET ESTABLISHED**

This record defines a security validation target.

It does not claim that every unknown-input scenario has already passed.

---

# 15. Kali Linux Security Finding Attribution

Kali Linux may be used to generate controlled adversarial traffic against an authorized VRP test environment.

However, the operating system used to execute an attack is not itself evidence of a security defect or successful defense.

Every Kali-related finding must identify:

    KALI_VERSION

    ATTACK_TOOL

    ATTACK_TOOL_VERSION

    TARGET_SOURCE_REVISION

    NETWORK_TOPOLOGY

    ATTACK_CLASS

    ATTACK_PARAMETERS

    EXECUTION_DURATION

    OBSERVED_RUNTIME_BEHAVIOR

    CANONICAL_STATE_RESULT

    POSITIVE_CONTROL

    EVIDENCE_REFERENCE

    FINAL_VERDICT

## 15.1 Supported report categories

- Malformed traffic.
- Replay pressure.
- Endpoint manipulation.
- Resource pressure.
- Compound disruption.

## 15.2 Evidence policy

A historical Kali Linux test may be referenced as engineering activity.

A formal PASS or FAIL requires the corresponding execution evidence.

Where such evidence is unavailable for public review, the result must remain:

    EVIDENCE PENDING

---

# 16. Security Regression Contract

A corrected security finding must be tested against both prohibited and legitimate behavior.

## 16.1 Negative control

The original prohibited operation must remain rejected.

## 16.2 Positive control

The corresponding legitimate operation must remain functional where required.

## 16.3 Canonical integrity

Rejected operations must not create unauthorized persistent-state changes.

## 16.4 Concurrency

Where relevant, the correction must remain valid under concurrent execution.

## 16.5 Recovery

Where relevant, the correction must remain valid across disruption and recovery.

## 16.6 Evidence integrity

The result must be attributable to the corrected revision.

A regression suite must not silently substitute tests from an unrelated source revision.

---

# 17. Security Fix Closure Gates

A finding may receive CLOSED — VERIFIED only when all applicable gates pass.

## GATE 1 — Finding Identity

The finding has a stable identifier.

## GATE 2 — Security Contract

The violated invariant is explicitly defined.

## GATE 3 — RED Evidence

The original failure is reproducibly established.

## GATE 4 — Correction Identity

The corrected source revision is identified.

## GATE 5 — GREEN Evidence

The original scenario passes against the corrected revision.

## GATE 6 — Regression Evidence

Relevant regression tests pass.

## GATE 7 — Positive Control

Legitimate behavior remains valid.

## GATE 8 — Evidence Integrity

The evidence is attributable and internally consistent.

## GATE 9 — Residual Risk

Known limitations are documented.

## GATE 10 — Review Decision

An authorized reviewer records the closure decision.

If a required gate is incomplete, the finding must not be silently classified as verified closed.

---

# 18. Evidence Bundle Structure

Recommended public remediation evidence structure:

    evidence/
      findings/
        VRP-FND-001/
          FINDING.md
          RED_EVIDENCE.md
          CORRECTION.md
          GREEN_EVIDENCE.md
          REGRESSION.md
          VERDICT.md
          SHA256SUMS

Each evidence record should identify the exact source revision and execution environment.

The public bundle must not contain secret keys, confidential credentials or protected implementation details.

---

# 19. Source and Release Provenance

Every formal remediation record should include:

- Source repository.
- Branch or release reference.
- Full commit SHA.
- Source tree SHA.
- Evaluated build identity.
- Test execution timestamp.
- Test environment.
- Test command.
- Exit status.
- Evidence artifact references.

A short commit prefix is useful for human navigation but is insufficient as a standalone immutable release identity.

Where a full commit SHA is available, it should be preferred.

A historical checkpoint must not be represented as a production release without corresponding release qualification.

---

# 20. Public Disclosure Policy

Public security remediation documentation may disclose:

- Affected invariant.
- Failure category.
- Observable impact.
- Corrective engineering objective.
- Test methodology.
- Source checkpoint.
- Regression outcome.
- Residual limitations.

Public documentation must not disclose:

- Protected Core algorithms.
- Proprietary authority-resolution logic.
- Internal state-machine implementation.
- Secret cryptographic material.
- Private operational credentials.
- Confidential deployment configurations.
- Non-public exploit details.

**PROTECTED CORE DISCLOSURE: NONE**

The purpose of the public register is accountability, not unrestricted implementation disclosure.

---

# 21. Independent Review Procedure

An independent reviewer should:

1. Select a finding ID.
2. Identify the affected invariant.
3. Inspect the original RED evidence.
4. Verify the affected source revision.
5. Inspect the correction checkpoint.
6. Execute the documented GREEN regression.
7. Verify the positive control.
8. Inspect canonical-state postconditions.
9. Verify evidence integrity.
10. Record a scoped closure verdict.

If the protected runtime is not available for independent execution, the reviewer must state the resulting assurance limitation.

Public reference-model results must not be substituted for private runtime correction evidence.

---

# 22. Residual Risk and Reopening Policy

A previously closed finding should be reopened when:

- The original violation reappears.
- A regression test fails.
- A related authority boundary is compromised.
- New evidence invalidates the original closure.
- A correction introduces an equivalent security violation.
- The finding's original assumptions no longer hold.

Reopening must preserve the original finding history.

Historical RED and GREEN records must not be silently overwritten.

---

# 23. Enterprise Due Diligence Requirements

An enterprise reviewer should be able to determine:

- What security property was violated.
- How the violation was demonstrated.
- Which source revision was affected.
- Which revision contains the correction.
- Whether the original test now passes.
- Whether related legitimate behavior remains functional.
- Whether residual risk remains.
- Whether the result has been independently reproduced.

These questions must be answerable without requiring unrestricted disclosure of proprietary algorithms.

A credible remediation record must preserve both engineering accountability and intellectual-property boundaries.

---

# 24. Assurance Limitations

This register does not establish:

- Absence of undiscovered vulnerabilities.
- Universal resistance to network attacks.
- Complete denial-of-service immunity.
- Formal verification of every runtime component.
- Third-party security certification.
- Qualification for all telecom environments.
- Qualification for safety-critical control systems.
- Production readiness of every historical checkpoint.

Each conclusion must remain bounded by its supporting evidence.

---

# 25. Related Public Documentation

## Security Assurance Case

    docs/review/VRP_SECURITY_ASSURANCE_CASE.md

## Evidence Traceability Matrix

    docs/review/VRP_EVIDENCE_TRACEABILITY_MATRIX.md

## Adversarial Security Validation Report

    docs/security/VRP_ADVERSARIAL_SECURITY_VALIDATION_REPORT.md

## Threat Model and Trust Boundaries

    docs/security/VRP_THREAT_MODEL_AND_TRUST_BOUNDARIES.md

## Public Validation Kit

https://github.com/Endless33/vrp-validation-kit

## VRP Architecture

https://github.com/Endless33/Veil-Routing-Protocol

---

# 26. Final Engineering Statement

Security engineering maturity is not demonstrated by claiming that a system has never failed.

It is demonstrated by the ability to identify a violated invariant, reproduce the failure, correct the behavior and verify that the correction does not weaken the surrounding security contract.

VRP's remediation philosophy is based on:

**REPRODUCIBLE FAILURE**

**SCOPED CORRECTION**

**VERIFIABLE GREEN**

**REGRESSION PRESERVATION**

**SOURCE ATTRIBUTION**

**EVIDENCE INTEGRITY**

**EXPLICIT RESIDUAL RISK**

The objective is not to hide historical failures.

The objective is to make corrective engineering accountable and technically reviewable.

**RED IS EVIDENCE.**

**GREEN REQUIRES VERIFICATION.**

**CLOSURE REQUIRES PROOF.**

**SESSION != TRANSPORT.**

---

**END OF DOCUMENT**

**Veil Routing Protocol — VRP**

**Author:** Vitalijus Riabovas

© 2026 Vitalijus Riabovas. All rights reserved.

This public document describes security findings, engineering checkpoints and verification requirements. It does not disclose proprietary implementation mechanisms or grant access to Protected Core materials. Applicable repository license terms govern reuse.
