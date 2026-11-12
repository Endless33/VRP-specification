
# VRP SECURITY CLAIMS, EVIDENCE LIMITATIONS & RELEASE PROVENANCE

## Veil Routing Protocol (VRP)
### Public Assurance Governance, Evidence Attribution and Verifiable Release Identity

**Document ID:** VRP-SCELRP-010  
**Version:** 1.0  
**Date:** 2026-11-12  
**Status:** Public Assurance Governance Specification — Proposed  
**Classification:** PUBLIC — NON-PROPRIETARY  
**Author:** Vitalijus Riabovas  
**Project:** Veil Routing Protocol / Jumping VPN

---

# 1. Executive Summary

Veil Routing Protocol (VRP) is an experimental continuity-first networking architecture based on the principle:

**SESSION != TRANSPORT**

VRP distinguishes logical-session identity from the lifetime of individual network transports.

The project defines security requirements addressing:

- Logical-session continuity.
- Transport-independent session identity.
- Replay containment.
- Duplicate-commit prevention.
- Execution authority integrity.
- Stale authority rejection.
- Epoch rollback rejection.
- Canonical-state integrity.
- Recovery integrity.
- Endpoint adoption safety.
- Fail-closed admission.
- Concurrent execution.
- Evidence integrity.
- Bounded runtime behavior.

These requirements form a public security contract.

However, defining a security contract is not equivalent to demonstrating that every implementation satisfies it.

Likewise, successful testing of a reference model does not automatically establish the correctness of an integrated runtime or production deployment.

This document defines the governance requirements for public VRP security claims, evidence attribution, release provenance and assurance limitations.

Its central principle is:

**EVERY SECURITY CLAIM MUST BE BOUNDED BY THE EVIDENCE THAT SUPPORTS IT.**

This document establishes a proposed framework for technically defensible public communication and independent review.

It does not claim that VRP has obtained formal certification, completed every proposed qualification scenario or received production authorization.

---

# 2. Purpose

The purpose of this document is to ensure that published VRP security statements are:

1. Technically precise.
2. Attributable to identifiable artifacts.
3. Supported by reproducible evidence.
4. Limited to the evaluated conditions.
5. Consistent with the public protocol specification.
6. Explicit about unresolved risks.
7. Distinguishable from architectural objectives.
8. Suitable for independent technical review.

The framework applies to:

- Public documentation.
- GitHub releases.
- Validation reports.
- Security advisories.
- Engineering presentations.
- Enterprise pilot materials.
- Technical due diligence.
- Public benchmark statements.
- External review responses.

---

# 3. Normative Language

The terms MUST, MUST NOT, SHOULD, SHOULD NOT and MAY are used as described in RFC 2119 and RFC 8174 when capitalized.

These terms define requirements for a publication or release claiming conformance to this assurance framework.

They do not automatically establish that every historical project artifact already satisfies the framework.

---

# 4. Foundational Assurance Principles

## 4.1 Claim precision

**VRP-ASSUR-001**

Every material security claim MUST identify the property being asserted.

## 4.2 Evidence attribution

**VRP-ASSUR-002**

A claim of observed behavior MUST identify the evaluated artifact or clearly disclose that attribution is unavailable.

## 4.3 Scope preservation

**VRP-ASSUR-003**

Evidence from one test scope MUST NOT be represented as proof of a broader scope without additional justification.

## 4.4 Independent verification

**VRP-ASSUR-004**

A result MUST NOT be described as independently verified unless an identifiable independent evaluation supports that description.

## 4.5 Release identity

**VRP-ASSUR-005**

A release-specific security claim MUST be associated with an immutable release identity.

## 4.6 Negative evidence

**VRP-ASSUR-006**

Known failures and unresolved mandatory test conditions MUST NOT be concealed from the corresponding assurance conclusion.

## 4.7 Protected information

**VRP-ASSUR-007**

Public assurance artifacts MUST NOT disclose confidential implementation mechanisms, credentials or protected cryptographic material.

## 4.8 Reproducibility

**VRP-ASSUR-008**

A claim described as reproducible MUST identify the necessary test procedure, artifact and execution conditions.

---

# 5. Security Claim Taxonomy

Public statements should be classified before publication.

| Class | Claim type | Meaning |
|---|---|---|
| C0 | Architectural Objective | Intended property or design goal |
| C1 | Normative Contract | Requirement specified by the protocol |
| C2 | Reference-Model Observation | Behavior demonstrated by an identified model |
| C3 | Runtime Observation | Behavior demonstrated by an identified runtime |
| C4 | Integrated-System Observation | Behavior demonstrated in an identified integration |
| C5 | Independent Reproduction | Result reproduced by an identifiable independent evaluator |
| C6 | Deployment Qualification | Result accepted for a specific deployment under agreed criteria |
| C7 | Formal Certification | Certification under an identified external framework |

The claim class MUST be supported by the actual evidence.

A C1 requirement MUST NOT be represented as a C4 implementation result.

A C2 reference-model PASS MUST NOT be represented as C6 deployment qualification.

A C5 independent reproduction MUST identify the independent evaluator and the evaluated artifact.

A C7 certification claim MUST identify the certification authority, applicable standard, scope and validity.

---

# 6. Claim Status Vocabulary

Every tracked claim SHOULD use one of the following statuses.

| Status | Definition |
|---|---|
| PROPOSED | Property defined as a design objective |
| SPECIFIED | Property defined in an applicable normative contract |
| TEST DESIGNED | Verification procedure documented |
| TEST EXECUTED | Test executed with attributable results |
| VERIFIED IN MODEL | Applicable reference-model test passed |
| VERIFIED IN RUNTIME | Applicable runtime test passed |
| VERIFIED IN INTEGRATION | Applicable integrated-system test passed |
| INDEPENDENTLY REPRODUCED | Independent evaluator reproduced the result |
| QUALIFIED FOR DEPLOYMENT | Deployment-specific acceptance completed |
| FAILED | Applicable security criterion violated |
| INCONCLUSIVE | Evidence insufficient |
| NOT ASSESSED | No applicable evaluation established |
| SUPERSEDED | Claim replaced by a newer version |
| WITHDRAWN | Claim no longer supported or authorized |

Claim status MUST be associated with the relevant artifact and evaluation scope.

A claim may have different statuses across different implementations.

---

# 7. Claim Registry Schema

Every material public security claim SHOULD have a stable identifier.

Recommended schema:

    CLAIM_ID

    CLAIM_TITLE

    CLAIM_CLASS

    CLAIM_STATUS

    NORMATIVE_REFERENCE

    AFFECTED_INVARIANT

    EVALUATED_ARTIFACT

    SOURCE_REVISION

    TEST_ID

    TEST_ENVIRONMENT

    EVIDENCE_REFERENCE

    EVIDENCE_INTEGRITY

    REVIEWER

    REVIEW_DATE

    LIMITATIONS

    EXPIRATION_OR_REVALIDATION_TRIGGER

    PUBLICATION_APPROVAL

Unknown values MUST NOT be fabricated.

Where a required field is unavailable, the claim MUST disclose the limitation.

---

# 8. Foundational VRP Claim

## Claim ID

VRP-CLM-001

## Statement

**SESSION != TRANSPORT**

## Architectural meaning

Logical-session identity is conceptually distinct from the lifetime of an individual transport connection.

## Required verification

A runtime continuity claim requires:

- Identified logical-session identity.
- Identified transport identity.
- Controlled transport disruption.
- Independent session-identity observation.
- Authority integrity verification.
- Canonical-state integrity verification.
- Application behavior measurement where applicable.

## Insufficient evidence

The following observations are insufficient by themselves:

- Process survival.
- Socket survival.
- UDP reconnection.
- Tunnel restoration.
- Endpoint replacement.
- Successful packet delivery.

## Claim boundary

A public architectural specification may establish the intended distinction.

It does not independently prove that a particular runtime preserves the original logical session under all network conditions.

---

# 9. Authority Integrity Claims

## Claim ID

VRP-CLM-002

## Statement

Transport reachability must not independently grant execution authority.

## Required evidence

- Unauthorized endpoint submission.
- Authority admission result.
- Protected-state postcondition.
- Legitimate positive control.
- Evaluated artifact identity.

## Acceptance criteria

    UNAUTHORIZED_AUTHORITY_ACCEPTED = FALSE

    UNAUTHORIZED_CANONICAL_COMMIT = FALSE

## Limitation

A passing endpoint rejection test does not automatically establish resistance to all forms of credential compromise or privileged attacker behavior.

---

# 10. Replay Containment Claims

## Claim ID

VRP-CLM-003

## Statement

Invalid replayed operations must not produce new protected execution.

## Required evidence

- Replay scenario definition.
- Original operation context.
- Replay attempt count.
- Acceptance and rejection counts.
- Canonical-state observations.
- Evaluated artifact identity.
- Positive-control behavior.

## Acceptance criteria

    INVALID_REPLAY_ACCEPTED = 0

    UNAUTHORIZED_DUPLICATE_COMMITS = 0

## Public reference-model evidence

The public VRP validation kit includes a replay-storm reference scenario involving 9,999 duplicate attempts.

Repository:

https://github.com/Endless33/vrp-validation-kit

The actual command, expected output and source revision must be verified against the selected published artifact.

## Claim boundary

A passing reference-model replay test establishes only the evaluated reference-model behavior.

It does not automatically establish integrated runtime enforcement.

---

# 11. Canonical-State Integrity Claims

## Claim ID

VRP-CLM-004

## Statement

Rejected operations must not produce unauthorized protected-state mutations.

## Required evidence

    CANONICAL_STATE_BEFORE

    INPUT_CLASSIFICATION

    ADMISSION_DECISION

    CANONICAL_STATE_AFTER

## Acceptance criteria

The protected-state postcondition must satisfy the applicable canonical-state contract.

## Limitation

A rejection log alone is insufficient.

The evaluator must inspect the relevant protected-state outcome.

---

# 12. Recovery Integrity Claims

## Claim ID

VRP-CLM-005

## Statement

Recovery must not bypass replay protection, restore invalid authority or corrupt canonical history.

## Required evidence

- Defined disruption.
- Initial logical-session identity.
- Initial authority state.
- Recovery event.
- Replay observations.
- Final authority state.
- Canonical-state postcondition.
- Independent session-identity result.

## Claim boundary

Successful connectivity restoration is not equivalent to verified recovery integrity.

A recovery test may pass an availability criterion while failing a security criterion.

These outcomes must remain distinct.

---

# 13. Autonomous Defense Claims

## Claim ID

VRP-CLM-006

## Statement

Unknown or malformed input must not acquire protected execution authority by default.

## Required evidence

- Input corpus identity.
- Test configuration.
- Admission decisions.
- Canonical-state postconditions.
- Resource measurements where applicable.
- Positive controls.

## Limitation

Passing a finite malformed-input corpus does not establish universal resistance to arbitrary malicious traffic.

The claim must identify the evaluated input classes.

---

# 14. Concurrency Claims

## Claim ID

VRP-CLM-007

## Statement

Concurrent admission must preserve the applicable single-admission contract.

## Required evidence

- Concurrent operation count.
- Accepted operation count.
- Rejected operation count.
- Duplicate commit count.
- Protected-state postcondition.
- Repeated execution results.

## Important distinction

Race-detector success is not equivalent to proof of linearizability.

The externally observable admission contract must also be evaluated.

---

# 15. Hostile-Network Continuity Claims

## Claim ID

VRP-CLM-008

## Statement

Supported transport disruption must not automatically replace the original logical-session identity.

## Proposed evaluation profiles

- Endpoint churn every 2 seconds.
- Packet loss at 30%.
- Packet loss at 50%.
- Packet loss at 80%.
- Packet loss at 90%.
- Blackout for 10 seconds.
- Blackout for 30 seconds.
- Blackout for 60 seconds.
- RTT between 300 and 900 milliseconds.
- Controlled jitter and reordering.
- Replay during migration.
- Competing authority recovery.
- Compound churn, loss, blackout and replay.

## Required evidence

Every profile must have its own attributable execution record.

## Limitation

These profiles are qualification targets.

Their inclusion in a specification does not imply completed testing or successful results.

---

# 16. Claim-to-Evidence Traceability

A formal assurance statement must maintain traceability.

Recommended relationship:

    SECURITY CLAIM
          |
          v
    NORMATIVE REQUIREMENT
          |
          v
    SECURITY INVARIANT
          |
          v
    TEST SCENARIO
          |
          v
    EVALUATED ARTIFACT
          |
          v
    OBSERVED POSTCONDITION
          |
          v
    EVIDENCE BUNDLE
          |
          v
    REVIEW VERDICT

A broken traceability chain limits the strength of the claim.

The absence of a test must not be interpreted as evidence of success.

---

# 17. Evidence Classification

The project may use the following evidence classes.

| Evidence level | Description |
|---|---|
| E0 | Public specification |
| E1 | Executable reference-model evidence |
| E2 | Controlled runtime evidence |
| E3 | Integrated-system evidence |
| E4 | Independent reproduction evidence |
| E5 | Deployment-specific qualification evidence |

Evidence level and claim class are related but not identical.

A specification may support a normative contract.

It does not establish runtime conformance.

An independent reproduction may support a specific runtime observation.

It does not establish universal security.

---

# 18. Evidence Quality Requirements

Every formal test result SHOULD include:

    EVIDENCE_ID

    SOURCE_REPOSITORY

    SOURCE_COMMIT

    SOURCE_TREE

    BUILD_ID

    TEST_ID

    TEST_CONFIGURATION

    EXECUTION_ENVIRONMENT

    EXECUTION_TIMESTAMP

    EXECUTION_COMMAND

    EXIT_STATUS

    EXPECTED_RESULT

    OBSERVED_RESULT

    SECURITY_POSTCONDITIONS

    POSITIVE_CONTROL

    VERDICT

    LIMITATIONS

The evidence must be sufficient to determine which implementation and conditions produced the result.

---

# 19. Evidence Integrity

Evidence integrity is distinct from evidence authenticity.

## 19.1 Integrity

Integrity verification determines whether evidence matches a recorded reference.

A cryptographic digest may support this property.

## 19.2 Authenticity

Authenticity requires confidence in the identity of the evidence producer.

A checksum alone does not establish producer identity.

## 19.3 Attribution

Attribution connects the evidence to a specific execution, artifact and evaluator.

## 19.4 Reproducibility

Reproducibility requires sufficient information to repeat the relevant evaluation.

## 19.5 Independent confirmation

Independent confirmation requires an identifiable evaluator who actually reproduced the relevant result.

These properties must not be treated as interchangeable.

---

# 20. Source Provenance

Source provenance establishes the identity of the evaluated source material.

Every formal source-qualified claim SHOULD record:

    REPOSITORY_URL

    COMMIT_SHA

    TREE_SHA

    RELEASE_TAG

    SOURCE_ARCHIVE_DIGEST

    SOURCE_RETRIEVAL_DATE

A release tag alone may be insufficient if it can be moved or replaced.

A commit SHA identifies a Git object within the applicable repository context.

A tree SHA identifies the corresponding tracked source tree.

Neither establishes that the source was built or tested unless additional evidence connects the source identity to the execution.

---

# 21. Build Provenance

Build provenance connects source material to a produced artifact.

Recommended fields:

    BUILD_ID

    SOURCE_COMMIT

    SOURCE_TREE

    BUILD_ENVIRONMENT

    TOOLCHAIN_VERSION

    DEPENDENCY_IDENTITY

    BUILD_CONFIGURATION

    BUILD_TIMESTAMP

    OUTPUT_ARTIFACT

    OUTPUT_SHA256

    BUILD_LOG_REFERENCE

## 21.1 Reproducible builds

Where reproducible builds are claimed, the build procedure and comparison method must be documented.

## 21.2 Non-reproducible builds

A build that is not bit-for-bit reproducible may still be attributable.

However, the limitation must be disclosed.

## 21.3 Build trust

A successful build does not establish that the resulting binary is secure.

Build provenance supports identity and traceability, not universal correctness.

---

# 22. Release Provenance

Release provenance connects an approved build to a published distribution artifact.

A formal release record SHOULD include:

    RELEASE_ID

    RELEASE_VERSION

    SOURCE_REPOSITORY

    SOURCE_COMMIT

    SOURCE_TREE

    BUILD_ID

    ARTIFACT_NAME

    ARTIFACT_SHA256

    RELEASE_TIMESTAMP

    RELEASE_CHANNEL

    RELEASE_APPROVAL

    EVIDENCE_BUNDLE_REFERENCE

    KNOWN_LIMITATIONS

    SECURITY_FINDINGS_STATUS

    SIGNATURE_REFERENCE

A missing optional signature must not be represented as a verified signed release.

---

# 23. Release Identity Invariants

A release process claiming immutable identity must preserve the following properties.

## REL-001 — Artifact identity

The release artifact must have a recorded content identity.

## REL-002 — Source attribution

The release must identify its source revision.

## REL-003 — Build attribution

The release must identify its build context.

## REL-004 — Integrity verification

The published artifact must match its recorded integrity reference.

## REL-005 — Publication consistency

A release identifier must not silently refer to different content.

## REL-006 — Evidence association

Security evidence must identify the release or artifact actually tested.

## REL-007 — Replacement transparency

Any replacement or withdrawal must be explicitly recorded.

## REL-008 — Verification independence

The recipient should be able to verify artifact integrity without relying solely on a human-readable release description.

These are release governance requirements.

Their inclusion does not establish that every historical VRP artifact already satisfies them.

---

# 24. Release Manifest Template

A recommended public release manifest is:

    VRP_RELEASE_MANIFEST

    Release ID:
    Release Version:
    Source Repository:
    Source Commit:
    Source Tree:
    Build ID:
    Toolchain:
    Target Platform:
    Artifact Name:
    Artifact SHA-256:
    Release Timestamp:
    Evidence Bundle:
    Known Limitations:
    Open Security Findings:
    Approval Status:
    Signature Reference:

Fields must be populated using actual release data.

Placeholder values must not be published as completed provenance records.

---

# 25. Release Verification Procedure

A recipient should be able to perform the following checks.

## Step 1 — Identify the release

Confirm the release identifier and intended artifact.

## Step 2 — Verify the artifact digest

Compute the artifact's cryptographic digest and compare it with the published manifest.

## Step 3 — Verify source identity

Inspect the recorded source revision where access is available.

## Step 4 — Verify build attribution

Inspect the available build metadata.

## Step 5 — Verify evidence association

Determine whether the published tests actually evaluated the same artifact.

## Step 6 — Inspect limitations

Review known security findings, exclusions and unresolved qualification conditions.

## Step 7 — Record the result

Publish a scoped verification outcome.

A successful digest comparison does not independently establish software authenticity or security correctness.

---

# 26. Release-to-Evidence Binding

A security claim must not silently migrate between releases.

For example:

    RELEASE_A -> TEST_A -> PASS

does not automatically imply:

    RELEASE_B -> PASS

A later release may contain changes affecting the tested property.

The project must establish one of the following:

1. Direct verification of the later release.
2. An explicitly justified equivalence argument.
3. A documented regression qualification.
4. An appropriately limited claim.

Any equivalence argument must identify the relevant assumptions and differences.

---

# 27. Evidence Freshness

Security evidence may become stale when:

- The source revision changes.
- The build configuration changes.
- Dependencies change.
- The operating system changes.
- The runtime environment changes.
- The threat model expands.
- New vulnerabilities are discovered.
- Relevant security requirements change.

## Required behavior

A claim must be reassessed when its supporting assumptions no longer hold.

A historical PASS may remain a valid historical observation while becoming insufficient for a newer release.

The project should distinguish:

    HISTORICAL_RESULT

from:

    CURRENT_RELEASE_QUALIFICATION

---

# 28. Historical Engineering Checkpoints

The VRP engineering history includes selected security-related checkpoints.

These references are provided for traceability.

They are not automatically public release attestations.

## 28.1 Rejected endpoint adoption

**Repository:** vrp-stage9s-validation

**Recorded commit:**

    4cfc77abf81a700b379ead63f0fa14814cdbb419

**Recorded engineering subject:**

    Prevent rejected endpoint path adoption.

## 28.2 Replay admission linearizability

**Repository:** jumping-vpn-core

**Recorded commit:**

    d7806b4b14998a97c25b5b86e353c6870da844ca

**Recorded engineering subject:**

    Replay linearizability hardening.

## 28.3 Recovery report consistency

**Repository:** vrp-stage9s-validation

**Recorded commit prefix:**

    f2c697c0

**Recorded engineering subject:**

    Snapshot-coherent recovery reporting.

## 28.4 Canonical aggregation reporting

**Repository:** vrp-stage9s-validation

**Recorded commit prefix:**

    702addcb

**Recorded engineering subject:**

    Removal of unsupported recovery proof capability.

## 28.5 Combined security boundary verification

**Project:** VRP Sovereign Guardian

**Recorded stage:** Stage 60

**Recorded internal outcome:**

    STAGE_60=VERIFIED_COMBINED_GREEN

These historical records require repository-specific verification before being used as formal public provenance evidence.

A private commit must not be described as publicly independently inspectable unless the relevant material is actually available.

Internal verification must not be represented as third-party certification.

---

# 29. Security Finding Provenance

Every formal security finding should preserve its historical identity.

Recommended fields:

    FINDING_ID

    DISCOVERY_DATE

    AFFECTED_ARTIFACT

    AFFECTED_REVISION

    SECURITY_INVARIANT

    RED_EVIDENCE

    CORRECTION_REVISION

    GREEN_EVIDENCE

    REGRESSION_EVIDENCE

    RESIDUAL_RISK

    CLOSURE_STATUS

A finding must not be marked verified closed solely because a correction commit exists.

The original failure and corrected behavior must be attributable.

---

# 30. RED-to-GREEN Assurance

The recommended security correction lifecycle is:

    INSPECT
       |
       v
    DEFINE INVARIANT
       |
       v
    RED — REPRODUCE FAILURE
       |
       v
    ATTRIBUTE ROOT CAUSE
       |
       v
    APPLY SCOPED CORRECTION
       |
       v
    GREEN — VERIFY CORRECTION
       |
       v
    RUN REGRESSIONS
       |
       v
    VERIFY EVIDENCE
       |
       v
    RECORD RESIDUAL RISK
       |
       v
    CLOSE OR RETAIN FINDING

A test returning exit code zero is not sufficient if the required security postconditions were not evaluated.

The project must preserve the distinction between test execution success and security verification success.

---

# 31. Negative Evidence Policy

Failed and inconclusive tests are part of the engineering record.

They must not be silently discarded.

## 31.1 Failed tests

A failed mandatory security test must be recorded as a failure.

## 31.2 Inconclusive tests

An inconclusive test must not be converted into PASS.

## 31.3 Harness failures

A test infrastructure failure must be distinguished from a demonstrated protocol defect.

## 31.4 Missing observations

A missing mandatory security observation must prevent an unqualified PASS.

## 31.5 Retesting

A later successful test must not erase the historical failure record.

The relationship between the original failure, correction and subsequent verification should remain traceable.

---

# 32. Public Security Language Policy

The project should use language consistent with the supporting evidence.

## 32.1 Architectural objective

Acceptable:

"VRP is designed to preserve logical-session identity independently of individual transport connections."

## 32.2 Normative requirement

Acceptable:

"The VRP security contract requires invalid replayed operations to be rejected."

## 32.3 Reference-model result

Acceptable when verified:

"The identified public reference model rejected 9,999 duplicate attempts under the documented test conditions."

## 32.4 Runtime result

Acceptable when verified:

"The identified runtime revision satisfied the replay-containment acceptance criteria under the documented conditions."

## 32.5 Independent result

Acceptable only with independent evidence:

"An independent evaluator reproduced the specified result against the identified artifact."

## 32.6 Deployment qualification

Acceptable only with deployment evidence:

"The identified deployment configuration satisfied the agreed pilot acceptance criteria."

Each statement must retain its scope.

---

# 33. Prohibited Unsupported Claims

The following statements must not be published as factual conclusions without corresponding evidence.

- "VRP is unbreakable."
- "VRP is immune to all replay attacks."
- "VRP guarantees zero downtime."
- "VRP guarantees continuity under every network failure."
- "VRP provides universal exactly-once execution."
- "VRP is fully certified."
- "VRP has been independently audited" when no such audit occurred.
- "VRP is production-ready for all telecom networks."
- "All security vulnerabilities have been eliminated."
- "The Protected Core is formally verified" without an applicable formal verification record.

Marketing language must not override technical evidence requirements.

---

# 34. Benchmark Claim Governance

Performance claims require their own provenance.

A benchmark statement SHOULD identify:

    BENCHMARK_ID

    EVALUATED_ARTIFACT

    HARDWARE

    OPERATING_SYSTEM

    NETWORK_TOPOLOGY

    WORKLOAD

    IMPAIRMENT_PROFILE

    SAMPLE_SIZE

    MEASUREMENT_METHOD

    OBSERVED_RESULTS

    STATISTICAL_SUMMARY

    LIMITATIONS

## 34.1 Recovery-time claims

Recovery time must specify its start and end events.

## 34.2 Throughput claims

Throughput must identify workload and measurement conditions.

## 34.3 Latency claims

Latency claims should report distributions rather than isolated best-case observations.

## 34.4 Comparison claims

Comparisons with other protocols must use comparable workloads and transparent methodology.

A benchmark from one environment must not be presented as a universal performance guarantee.

---

# 35. Security and Availability Separation

Security integrity and availability are different dimensions.

A system may:

- Reject malicious traffic correctly.
- Preserve authority integrity.
- Preserve canonical-state integrity.
- Fail to deliver application traffic within the required time.

Such a result may satisfy security-integrity criteria while failing availability criteria.

Conversely, a system may restore connectivity quickly while accepting unauthorized execution.

Such a result is a security failure regardless of recovery speed.

The project must report these dimensions separately.

---

# 36. Independent Review Attribution

An independent review record SHOULD contain:

    REVIEW_ID

    REVIEWER

    REVIEW_ORGANIZATION

    REVIEW_DATE

    REVIEW_SCOPE

    EVALUATED_ARTIFACT

    SOURCE_REVISION

    EXECUTED_TESTS

    VERIFIED_POSTCONDITIONS

    FINDINGS

    LIMITATIONS

    FINAL_VERDICT

The reviewer must have actually performed the stated evaluation.

An endorsement, informal comment or repository visit does not constitute an independent security audit.

---

# 37. Enterprise Pilot Claims

Enterprise pilot results must be tied to an identified evaluation.

Required information includes:

- Customer-approved scope.
- Evaluated artifact.
- Test environment.
- Acceptance criteria.
- Executed tests.
- Observed results.
- Open findings.
- Residual risks.
- Formal decision.
- Publication authorization.

A successful laboratory demonstration must not be described as completed enterprise qualification.

A completed pilot must not automatically be described as production authorization.

Customer identity and results must not be publicly disclosed without the necessary permissions.

---

# 38. Assurance Limitations

The public VRP assurance framework does not establish:

- Absence of undiscovered vulnerabilities.
- Universal resistance to network attacks.
- Complete denial-of-service immunity.
- Formal verification of all implementation components.
- Guaranteed session continuity under arbitrary failures.
- Exactly-once application execution in every integration.
- Universal compatibility with existing infrastructure.
- Telecom certification.
- Safety-critical certification.
- Production qualification across all platforms.
- Independent audit completion.
- Deployment approval by any particular organization.

These limitations must remain visible in relevant public security conclusions.

---

# 39. Known Sources of Assurance Uncertainty

Assurance uncertainty may arise from:

## 39.1 Limited test coverage

Only selected scenarios may have been evaluated.

## 39.2 Environment dependence

Results may depend on operating system, hardware or network conditions.

## 39.3 Instrumentation limitations

Required security postconditions may not be observable.

## 39.4 Source attribution gaps

The evaluated revision may not be sufficiently identified.

## 39.5 Reference-model separation

A public model may not exercise the integrated runtime path.

## 39.6 Incomplete independent reproduction

Author-produced evidence may not have been externally reproduced.

## 39.7 Changing threat assumptions

New attacker capabilities may invalidate earlier conclusions.

## 39.8 Deployment-specific behavior

A laboratory PASS may not transfer directly to a customer environment.

These limitations must be reflected in the final assurance verdict.

---

# 40. Assurance Decision Gates

A formal release-specific security claim should pass the following gates.

## GATE 1 — Claim Definition

The asserted property is precise.

## GATE 2 — Normative Reference

The applicable security contract is identified.

## GATE 3 — Artifact Identity

The evaluated artifact is uniquely identified.

## GATE 4 — Test Identity

The supporting test is identified.

## GATE 5 — Execution Evidence

The test was actually executed.

## GATE 6 — Postcondition Verification

The relevant security postconditions were observed.

## GATE 7 — Positive Control

Required legitimate behavior was verified.

## GATE 8 — Evidence Integrity

The evidence is attributable and internally consistent.

## GATE 9 — Limitation Disclosure

The scope and residual uncertainty are documented.

## GATE 10 — Publication Approval

The claim has been approved for the intended publication context.

A missing mandatory gate must prevent an unqualified verified claim.

---

# 41. Release Assurance Verdicts

A release assurance report should use explicit verdicts.

| Verdict | Meaning |
|---|---|
| VERIFIED WITHIN SCOPE | Applicable criteria satisfied with attributable evidence |
| VERIFIED IN REFERENCE MODEL | Selected model properties passed |
| VERIFIED IN RUNTIME | Selected runtime properties passed |
| VERIFIED IN INTEGRATION | Selected integrated-system properties passed |
| INDEPENDENTLY REPRODUCED | External reproduction completed |
| CONDITIONALLY QUALIFIED | Defined conditions remain |
| NOT QUALIFIED | Mandatory qualification not established |
| FAILED | Required criterion violated |
| INCONCLUSIVE | Evidence insufficient |
| NOT ASSESSED | No applicable assessment completed |

The verdict must identify its scope.

No verdict should imply broader certification than the evidence supports.

---

# 42. Public Release Assurance Record

A recommended release assurance record is:

    VRP RELEASE ASSURANCE RECORD

    Release ID:
    Release Version:
    Source Repository:
    Source Commit:
    Source Tree:
    Build ID:
    Artifact SHA-256:
    Security Contract Version:
    Threat Model Version:
    Executed Test Campaigns:
    Reference-Model Results:
    Runtime Results:
    Integrated-System Results:
    Independent Reproduction:
    Open Security Findings:
    Known Limitations:
    Evidence Bundle:
    Evidence Integrity Status:
    Final Assurance Verdict:
    Approval Authority:
    Approval Date:

The record becomes a formal release statement only after its required fields are completed and approved.

---

# 43. Suggested Repository Assurance Structure

A public assurance repository may organize its artifacts as follows:

    docs/
      assurance/
        VRP_SECURITY_CLAIMS_EVIDENCE_LIMITATIONS_AND_RELEASE_PROVENANCE.md

      review/
        VRP_SECURITY_ASSURANCE_CASE.md
        VRP_EVIDENCE_TRACEABILITY_MATRIX.md
        VRP_INDEPENDENT_SECURITY_AND_PROTOCOL_REVIEW_GUIDE.md

      security/
        VRP_ADVERSARIAL_SECURITY_VALIDATION_REPORT.md
        VRP_THREAT_MODEL_AND_TRUST_BOUNDARIES.md
        VRP_SECURITY_FINDINGS_AND_REMEDIATION_REGISTER.md
        VRP_AUTONOMOUS_DEFENSE_AND_FAIL_CLOSED_DECISION_CONTRACT.md

      validation/
        VRP_HOSTILE_NETWORK_CONTINUITY_QUALIFICATION_MATRIX.md

      enterprise/
        VRP_ENTERPRISE_PILOT_SECURITY_AND_ACCEPTANCE_CRITERIA.md

      releases/
        RELEASE_MANIFEST_TEMPLATE.md
        RELEASE_ASSURANCE_RECORD_TEMPLATE.md

    evidence/
      README.md

The release templates and evidence index shown above are optional future artifacts.

Their appearance in this suggested structure does not imply that they already exist.

---

# 44. Public Documentation Consistency

All public assurance documents should use consistent terminology.

The following distinctions must be preserved:

**Logical Session Identity**

The identity of the logical protocol session.

**Transport Identity**

The identity of the underlying communication path or transport context.

**Execution Authority**

Permission to perform protected operations.

**Canonical State**

The authoritative protected state relevant to the protocol contract.

**Replay Containment**

Prevention of invalid repeated protected execution.

**Recovery Integrity**

Preservation of applicable security invariants during recovery.

**Evidence Integrity**

Confidence that evidence has not been altered relative to its recorded integrity reference.

**Independent Reproduction**

Execution of the relevant evaluation by an identifiable independent party.

These concepts must not be used interchangeably.

---

# 45. Protected Core Disclosure Boundary

This document intentionally exposes:

- Public security claim definitions.
- Evidence classification.
- Assurance requirements.
- Release identity requirements.
- Verification procedures.
- Review criteria.
- Known limitations.
- Public documentation relationships.

This document intentionally excludes:

- Proprietary continuity algorithms.
- Internal authority-resolution mechanisms.
- Private state-machine implementation.
- Protected packet-processing logic.
- Confidential runtime source code.
- Secret cryptographic material.
- Private deployment configurations.
- Non-public security mechanisms.

**PROTECTED CORE DISCLOSURE: NONE**

Public assurance must remain meaningful without disclosing proprietary implementation details.

Restricted technical access, where appropriate, must be governed by separately agreed authorization and confidentiality conditions.

---

# 46. Public References

## VRP Project

https://github.com/Endless33/Veil-Routing-Protocol

## VRP Validation Kit

https://github.com/Endless33/vrp-validation-kit

## RFC 2119

Key words for use in RFCs to Indicate Requirement Levels.

https://www.rfc-editor.org/rfc/rfc2119

## RFC 8174

Ambiguity of Uppercase vs Lowercase in RFC 2119 Key Words.

https://www.rfc-editor.org/rfc/rfc8174

---

# 47. Relationship to the VRP Assurance Documentation Set

This document completes the proposed ten-document public assurance series.

## Document 1 — Security Assurance Case

    docs/review/VRP_SECURITY_ASSURANCE_CASE.md

Defines the overall public security assurance argument.

## Document 2 — Evidence Traceability Matrix

    docs/review/VRP_EVIDENCE_TRACEABILITY_MATRIX.md

Maps security invariants to validation requirements and evidence.

## Document 3 — Adversarial Security Validation Report

    docs/security/VRP_ADVERSARIAL_SECURITY_VALIDATION_REPORT.md

Defines adversarial test categories and evidence expectations.

## Document 4 — Threat Model and Trust Boundaries

    docs/security/VRP_THREAT_MODEL_AND_TRUST_BOUNDARIES.md

Defines protected assets, attacker capabilities and trust boundaries.

## Document 5 — Security Findings and Remediation Register

    docs/security/VRP_SECURITY_FINDINGS_AND_REMEDIATION_REGISTER.md

Defines security finding accountability and RED-to-GREEN remediation evidence.

## Document 6 — Autonomous Defense and Fail-Closed Decision Contract

    docs/security/VRP_AUTONOMOUS_DEFENSE_AND_FAIL_CLOSED_DECISION_CONTRACT.md

Defines public requirements for autonomous security decisions.

## Document 7 — Hostile Network Continuity Qualification Matrix

    docs/validation/VRP_HOSTILE_NETWORK_CONTINUITY_QUALIFICATION_MATRIX.md

Defines continuity and resilience qualification under controlled network impairment.

## Document 8 — Independent Security and Protocol Review Guide

    docs/review/VRP_INDEPENDENT_SECURITY_AND_PROTOCOL_REVIEW_GUIDE.md

Defines external review procedures and evidence-based verdicts.

## Document 9 — Enterprise Pilot Security and Acceptance Criteria

    docs/enterprise/VRP_ENTERPRISE_PILOT_SECURITY_AND_ACCEPTANCE_CRITERIA.md

Defines controlled enterprise pilot qualification and acceptance.

## Document 10 — Security Claims, Evidence Limitations and Release Provenance

    docs/assurance/VRP_SECURITY_CLAIMS_EVIDENCE_LIMITATIONS_AND_RELEASE_PROVENANCE.md

Defines claim governance, evidence limitations and verifiable release identity.

Together, these documents provide a structured public assurance framework.

Their existence does not replace execution evidence, independent review or deployment-specific qualification.

---

# 48. Final Assurance Statement

A serious protocol security claim must survive more than a successful demonstration.

It must survive questions about:

- What was tested.
- Which artifact was tested.
- Which security invariant was evaluated.
- Which attacker capabilities were represented.
- Which postconditions were observed.
- Whether the test can be reproduced.
- Whether the evidence is attributable.
- Whether the result applies to the claimed release.
- Whether the result was independently reproduced.
- Which limitations remain.

The VRP public assurance framework is based on the following distinctions:

**SESSION != TRANSPORT**

**RECONNECTION != VERIFIED CONTINUITY**

**REACHABILITY != AUTHORITY**

**REPLAY != NEW EXECUTION**

**REJECTION != PARTIAL COMMIT**

**RECOVERY != AUTHORITY RESET**

**REFERENCE MODEL != INTEGRATED RUNTIME**

**AUTHOR TESTING != INDEPENDENT CERTIFICATION**

**SOURCE COMMIT != VERIFIED RELEASE**

**ARTIFACT INTEGRITY != SOFTWARE SECURITY**

**TEST EXECUTION != SECURITY QUALIFICATION**

**SECURITY CLAIM != VERIFIED EVIDENCE**

The engineering objective is:

**PRESERVE LOGICAL-SESSION CONTINUITY WITHOUT COMPROMISING AUTHORITY, CANONICAL STATE OR EXECUTION INTEGRITY.**

The assurance objective is:

**MAKE EVERY MATERIAL SECURITY CLAIM TRACEABLE TO AN IDENTIFIED ARTIFACT, AN EXPLICIT TEST AND AN OBSERVED RESULT.**

The release objective is:

**PUBLISH IDENTIFIABLE ARTIFACTS WITH VERIFIABLE PROVENANCE AND HONESTLY SCOPED ASSURANCE.**

The long-term standard is:

**DEFINE THE INVARIANT.**

**IDENTIFY THE SOURCE.**

**VERIFY THE BUILD.**

**EXECUTE THE TEST.**

**OBSERVE THE POSTCONDITION.**

**PRESERVE THE EVIDENCE.**

**DECLARE THE LIMITATIONS.**

**PUBLISH ONLY WHAT THE EVIDENCE SUPPORTS.**

---

**END OF DOCUMENT**

**Veil Routing Protocol — VRP**

**Author:** Vitalijus Riabovas

© 2026 Vitalijus Riabovas. All rights reserved.

This public document defines security assurance governance, evidence attribution and release provenance requirements. It does not constitute certification, production authorization, a commercial agreement or disclosure of proprietary implementation mechanisms. Applicable repository license terms govern reuse.
