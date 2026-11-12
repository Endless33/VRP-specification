
# VRP ENTERPRISE PILOT SECURITY & ACCEPTANCE CRITERIA

## Veil Routing Protocol (VRP)
### Controlled Enterprise Evaluation, Security Qualification and Evidence-Based Acceptance

**Document ID:** VRP-EPSAC-009  
**Version:** 1.0  
**Date:** 2026-11-12  
**Status:** Public Enterprise Pilot Framework — Proposed  
**Classification:** PUBLIC — NON-PROPRIETARY  
**Author:** Vitalijus Riabovas  
**Project:** Veil Routing Protocol / Jumping VPN

---

# 1. Executive Summary

Veil Routing Protocol (VRP) is an experimental continuity-first networking architecture based on the principle:

**SESSION != TRANSPORT**

VRP is designed around the distinction between logical-session identity and the lifetime of an individual network transport.

This architectural distinction is relevant to environments where network paths may change, degrade or become temporarily unavailable.

Potential evaluation environments include:

- Enterprise networking.
- Distributed infrastructure.
- Mobile connectivity.
- Multi-access networks.
- Relay-based connectivity.
- Intermittent wireless systems.
- Satellite-like network conditions.
- Long-lived application sessions.

However, architectural relevance does not establish operational suitability.

Before VRP can be considered for deployment in a specific enterprise environment, its behavior must be evaluated against explicit technical, security and operational requirements.

This document defines a proposed framework for controlled enterprise pilot qualification.

It establishes:

1. Pilot scope.
2. Security prerequisites.
3. Acceptance criteria.
4. Test responsibilities.
5. Evidence requirements.
6. Failure classification.
7. Operational constraints.
8. Intellectual-property boundaries.
9. Exit criteria.
10. Formal acceptance decisions.

The central principle is:

**ENTERPRISE ACCEPTANCE MUST BE BASED ON OBSERVED, REPRODUCIBLE AND CONTRACTUALLY AGREED RESULTS.**

This document is a public evaluation framework.

It is not a statement that VRP has already completed a specific customer pilot, obtained certification or been approved for production deployment.

---

# 2. Purpose

The purpose of an enterprise pilot is to determine whether an identified VRP implementation can satisfy agreed requirements in a controlled target environment.

A pilot must produce sufficient evidence to support a technical decision.

That decision may be:

- Proceed to a broader evaluation.
- Proceed with conditions.
- Repeat selected qualification tests.
- Require remediation.
- Reject the proposed deployment.
- Terminate the evaluation.

A pilot must not be treated as a marketing demonstration with an automatically predetermined outcome.

---

# 3. Normative Language

The terms MUST, MUST NOT, SHOULD, SHOULD NOT and MAY are used as described in RFC 2119 and RFC 8174 when capitalized.

For this document, mandatory requirements apply to a pilot claiming conformance to the agreed evaluation framework.

The final pilot agreement may introduce additional requirements.

Any deviation from this framework must be explicitly documented and approved by the relevant parties.

---

# 4. Pilot Governance Principles

The enterprise pilot follows ten principles.

## 4.1 Defined scope

The evaluated environment and intended use case must be identified.

## 4.2 Immutable artifact identity

The evaluated build must be attributable to a specific source and release identity.

## 4.3 Security before availability

Recovery must not bypass security requirements to improve connectivity.

## 4.4 Independent session verification

Logical-session continuity must be verified independently of transport restoration.

## 4.5 Observable authority integrity

Unauthorized authority must not create protected execution.

## 4.6 Reproducible tests

Qualification procedures must be documented sufficiently for authorized repetition.

## 4.7 Explicit acceptance thresholds

Performance and availability targets must be agreed before testing.

## 4.8 Evidence preservation

Test results must remain attributable and internally consistent.

## 4.9 Controlled disclosure

The evaluation must respect intellectual-property and confidentiality boundaries.

## 4.10 No automatic production authorization

Successful pilot completion does not automatically authorize unrestricted production deployment.

---

# 5. Pilot Classification

A pilot should be assigned an explicit evaluation class.

| Class | Description |
|---|---|
| P0 — Technical Demonstration | Limited demonstration of selected behavior |
| P1 — Controlled Laboratory Pilot | Isolated testing under documented conditions |
| P2 — Enterprise Integration Pilot | Evaluation within a controlled enterprise environment |
| P3 — Pre-Production Qualification | Extended evaluation against deployment-specific requirements |
| P4 — Production Authorization Review | Separate operational and security approval process |

These classes are proposed evaluation categories.

They are not external certification levels.

Progression between classes requires a documented decision.

---

# 6. Pilot Scope Definition

Before execution, the parties MUST agree on the following.

    PILOT_ID

    CUSTOMER_ORGANIZATION

    TECHNICAL_OWNER

    SECURITY_OWNER

    EVALUATION_CLASS

    TARGET_USE_CASE

    DEPLOYMENT_ENVIRONMENT

    EVALUATED_ARTIFACT

    TEST_DURATION

    SECURITY_REQUIREMENTS

    PERFORMANCE_TARGETS

    AVAILABILITY_TARGETS

    DATA_CLASSIFICATION

    EVIDENCE_REQUIREMENTS

    ACCEPTANCE_AUTHORITY

    TERMINATION_CONDITIONS

Unknown values must be resolved before the affected test can be formally accepted.

---

# 7. Intended Enterprise Use Cases

VRP may be evaluated for use cases where transport instability affects long-lived logical sessions.

Potential examples include:

## 7.1 Mobile connectivity

Evaluation of logical-session behavior across controlled access-network transitions.

## 7.2 Multi-path enterprise networking

Evaluation of session continuity when an authorized transport path becomes unavailable.

## 7.3 Distributed services

Evaluation of recovery behavior where application interactions depend on persistent logical-session identity.

## 7.4 Intermittent connectivity

Evaluation under controlled blackout, packet loss and latency.

## 7.5 Relay-based networking

Evaluation of authorized transport replacement and recovery.

These examples describe potential evaluation contexts.

They do not constitute claims of certified interoperability or proven deployment readiness.

---

# 8. Pilot Entry Requirements

The following prerequisites must be satisfied before formal testing.

## ENTRY-001 — Authorized environment

The customer must authorize the test environment and all planned adversarial activities.

## ENTRY-002 — Artifact identity

The evaluated VRP build must have an attributable identity.

## ENTRY-003 — Test topology

The network topology must be documented.

## ENTRY-004 — Security assumptions

The applicable trust boundaries and attacker capabilities must be defined.

## ENTRY-005 — Session oracle

An independent logical-session identity verification method must be available for continuity qualification.

## ENTRY-006 — Application workload

The pilot must define representative application operations.

## ENTRY-007 — Measurement instrumentation

Required security, performance and recovery observations must be available.

## ENTRY-008 — Rollback plan

The customer must approve a safe termination and rollback procedure.

## ENTRY-009 — Data handling

Test data, logs and credentials must be handled under agreed security controls.

## ENTRY-010 — Acceptance thresholds

All mandatory acceptance thresholds must be agreed before formal execution.

If a mandatory entry condition is missing, the affected test must be classified as BLOCKED or NOT READY.

---

# 9. Artifact and Release Identity

Every evaluated artifact MUST be identifiable.

Required information:

    SOURCE_REPOSITORY

    SOURCE_COMMIT

    SOURCE_TREE

    RELEASE_VERSION

    BUILD_IDENTIFIER

    ARTIFACT_SHA256

    BUILD_TIMESTAMP

    TARGET_PLATFORM

    RUNTIME_VERSION

    CONFIGURATION_REFERENCE

The parties may use an opaque release identifier where source access is restricted.

However, the artifact must remain uniquely identifiable and its integrity independently checkable.

A mutable branch name is not sufficient as a release identity.

---

# 10. Delivery Integrity

Pilot delivery artifacts must be protected against accidental modification and unauthorized replacement.

The delivery process SHOULD support:

- Immutable artifact identity.
- Integrity manifests.
- Cryptographic checksums.
- Explicit release metadata.
- Atomic publication.
- Controlled access.
- Reproducible verification.
- Clear version attribution.

## 10.1 Required delivery check

    DELIVERY_ARTIFACT = IDENTIFIED

    INTEGRITY_CHECK = PASS

    RELEASE_METADATA = PRESENT

    CUSTOMER_RECEIPT = RECORDED

## 10.2 Important limitation

SHA-256 integrity checks do not independently authenticate the artifact producer.

Where publisher authenticity is required, an agreed cryptographic signing and trust-verification mechanism must be used.

---

# 11. Security Acceptance Domains

The pilot MUST identify which security domains are in scope.

| Domain | Required assessment |
|---|---|
| Session identity | Logical-session continuity |
| Authority integrity | Unauthorized authority rejection |
| Replay containment | Duplicate and stale operation rejection |
| Canonical integrity | Protected-state consistency |
| Endpoint integrity | Unauthorized path adoption prevention |
| Recovery integrity | Secure recovery behavior |
| Concurrency | Duplicate-commit prevention |
| Input validation | Malformed and unknown input handling |
| Resource behavior | Bounded operation under agreed load |
| Evidence integrity | Attributable qualification results |

An overall security PASS requires all mandatory in-scope domains to satisfy their acceptance criteria.

---

# 12. SEC-001 — Logical-Session Identity

## Objective

Verify that the original logical session remains identifiable across supported transport disruption.

## Required baseline

    SESSION_IDENTITY = S0

    TRANSPORT_IDENTITY = T0

## Required postcondition

After an authorized transport transition:

    SESSION_IDENTITY = S0

The transport identity may change.

## Acceptance criteria

- Independent session-identity observation.
- No unauthorized session replacement.
- Authority integrity preserved.
- Canonical-state integrity preserved.
- Application behavior measured.

## Rejection criteria

The test must not pass based solely on:

- Process survival.
- UDP reconnection.
- Socket restoration.
- Tunnel restoration.
- Endpoint replacement.
- Packet delivery.

If the session identity cannot be independently verified:

    SEC-001 = INCONCLUSIVE

---

# 13. SEC-002 — Authority Integrity

## Objective

Verify that protected execution remains subject to valid authority.

## Required negative scenarios

- Stale authority.
- Invalid authority.
- Unauthorized endpoint.
- Invalid epoch rollback.
- Conflicting authority where applicable.

## Acceptance criteria

    UNAUTHORIZED_AUTHORITY_ACCEPTED = 0

    UNAUTHORIZED_CANONICAL_COMMITS = 0

## Positive control

Legitimate authorized operations must remain possible under the agreed conditions.

## Failure condition

Any demonstrated unauthorized protected commit constitutes a security failure.

---

# 14. SEC-003 — Replay Containment

## Objective

Verify that invalid replayed operations do not produce new protected execution.

## Required tests

- Sequential duplicate submission.
- Historical replay.
- Replay during recovery.
- Concurrent duplicate admission where applicable.

## Acceptance criteria

    INVALID_REPLAY_ACCEPTED = 0

    UNAUTHORIZED_DUPLICATE_COMMITS = 0

## Evidence

The report must include:

- Replay attempt count.
- Rejected attempt count.
- Accepted attempt count.
- Protected-state observations.
- Source revision.
- Test configuration.

A public reference-model replay PASS does not substitute for integrated pilot runtime verification.

---

# 15. SEC-004 — Canonical-State Integrity

## Objective

Verify that rejected operations do not create unauthorized persistent changes.

## Required observations

    CANONICAL_STATE_BEFORE

    INPUT_CLASSIFICATION

    ADMISSION_DECISION

    CANONICAL_STATE_AFTER

## Acceptance criteria

Rejected operations must not create unauthorized canonical mutations.

The evaluator must inspect the protected-state postcondition.

A rejection log alone is insufficient.

---

# 16. SEC-005 — Endpoint Adoption Safety

## Objective

Verify that rejected traffic cannot authorize a new persistent endpoint.

## Required negative control

Submit traffic from an unauthorized candidate endpoint under controlled conditions.

## Acceptance criteria

    REJECTED_ENDPOINT_ADOPTED = FALSE

    UNAUTHORIZED_AUTHORITY_TRANSFER = FALSE

## Positive control

An authorized transport transition must remain possible where the pilot requires migration support.

---

# 17. SEC-006 — Recovery Integrity

## Objective

Verify that recovery preserves applicable security requirements.

## Required scenarios

- Short blackout.
- Extended blackout.
- Transport replacement.
- Delayed historical traffic.
- Replay during recovery.
- Authority contention where applicable.

## Acceptance criteria

Recovery must not:

- Reactivate invalid authority.
- Accept prohibited replay.
- Rewrite protected canonical history.
- Grant authority based solely on transport reachability.

The continuity outcome and security-integrity outcome must be reported separately.

---

# 18. SEC-007 — Concurrent Admission

## Objective

Verify that concurrent operations preserve the applicable admission contract.

## Required tests

- Concurrent duplicate submissions.
- Repeated execution.
- Protected-state verification.
- Legitimate progression after rejection.

## Acceptance criteria

Where single admission is required:

    UNAUTHORIZED_DUPLICATE_COMMITS = 0

## Important limitation

A race-detector PASS does not independently prove linearizability.

The externally observable contract must be verified.

---

# 19. SEC-008 — Malformed and Unknown Input

## Objective

Verify fail-closed handling of invalid network input.

## Test categories

- Malformed packets.
- Unsupported message types.
- Invalid lengths.
- Unexpected input sequences.
- Invalid authorization context.

## Acceptance criteria

    UNAUTHORIZED_EXECUTION = FALSE

    UNAUTHORIZED_CANONICAL_MUTATION = FALSE

    UNAUTHORIZED_ENDPOINT_ADOPTION = FALSE

## Additional observations

- Process stability.
- Resource consumption.
- Admission outcome.
- Legitimate positive-control traffic.

Passing a finite input corpus does not establish immunity to all malformed inputs.

---

# 20. SEC-009 — Resource Pressure

## Objective

Evaluate behavior under agreed resource-pressure conditions.

## Required measurements

- CPU utilization.
- Memory consumption.
- Input rate.
- Rejection rate.
- Queue utilization where available.
- Application progress.
- Runtime crashes.
- Recovery behavior.

## Acceptance criteria

The implementation must remain within the resource and security bounds agreed for the pilot.

Resource limits must be documented before execution.

A system may preserve security integrity while failing an availability target.

These results must be reported separately.

---

# 21. SEC-010 — Evidence Integrity

## Objective

Verify that the pilot results are attributable and internally consistent.

## Required evidence

    TEST_ID

    ARTIFACT_ID

    TEST_CONFIGURATION

    EXECUTION_TIMESTAMP

    EXPECTED_RESULT

    OBSERVED_RESULT

    EXIT_STATUS

    EVIDENCE_REFERENCE

    VERDICT

## Acceptance criteria

- Evidence is associated with the evaluated artifact.
- Required observations are present.
- Results are internally consistent.
- Evidence integrity checks pass.
- Missing information is disclosed.

A missing mandatory security observation must prevent an unqualified PASS.

---

# 22. Network Resilience Qualification

The pilot may include the following controlled impairment profiles.

| Test ID | Scenario | Proposed configuration |
|---|---|---|
| NET-001 | Endpoint churn | Every 2 seconds |
| NET-002 | Packet loss | 30% |
| NET-003 | Packet loss | 50% |
| NET-004 | Packet loss | 80% |
| NET-005 | Packet loss | 90% |
| NET-006 | Blackout | 10 seconds |
| NET-007 | Blackout | 30 seconds |
| NET-008 | Blackout | 60 seconds |
| NET-009 | High latency | RTT 300–900 ms |
| NET-010 | Reordering | Controlled |
| NET-011 | Replay during migration | Controlled |
| NET-012 | Competing authority recovery | Controlled |
| NET-013 | Compound disruption | Churn + 80% loss + blackout + replay |

These profiles are proposed qualification cases.

The customer and protocol provider must agree on which profiles are mandatory.

Inclusion in this table does not imply successful execution.

---

# 23. Compound Failure Acceptance

Compound failure testing evaluates the interaction between multiple disruptions.

## Proposed profile

    ENDPOINT_CHURN_INTERVAL = 2 SECONDS

    PACKET_LOSS = 80%

    BLACKOUT = ENABLED

    REPLAY_PRESSURE = ENABLED

    SESSION_ORACLE = REQUIRED

    AUTHORITY_OBSERVATION = REQUIRED

## Required postconditions

    UNAUTHORIZED_AUTHORITY_ACCEPTED = FALSE

    INVALID_REPLAY_ACCEPTED = FALSE

    UNAUTHORIZED_CANONICAL_COMMIT = FALSE

## Continuity acceptance

For a profile explicitly declared within the supported continuity envelope:

    SESSION_IDENTITY_AFTER = SESSION_IDENTITY_BEFORE

The exact blackout duration, replay intensity, execution duration and impairment ordering must be recorded.

A successful compound test does not establish success under every possible impairment combination.

---

# 24. Application-Level Acceptance

Transport recovery does not automatically establish application recovery.

The pilot must define representative application operations.

Required measurements SHOULD include:

    OPERATIONS_SUBMITTED

    OPERATIONS_RECEIVED

    OPERATIONS_ACKNOWLEDGED

    OPERATIONS_COMMITTED

    OPERATIONS_FAILED

    DUPLICATE_OPERATIONS

    APPLICATION_RECOVERY_TIME

## 24.1 Application semantics

The pilot must distinguish:

- Transport delivery.
- Application receipt.
- Application acknowledgement.
- Application commit.

Exactly-once application execution must not be claimed without a separately defined and verified execution contract.

---

# 25. Performance and Availability Targets

Performance and availability acceptance thresholds must be negotiated before formal testing.

The following table is a template, not a claim of VRP performance.

| Metric | Acceptance threshold | Measurement method |
|---|---|---|
| Session continuity | To be agreed | Independent session oracle |
| Transport recovery time | To be agreed | Defined recovery timestamps |
| Application recovery time | To be agreed | Application completion |
| Application delivery rate | To be agreed | Workload measurements |
| P95 operation latency | To be agreed | Timed application operations |
| P99 operation latency | To be agreed | Timed application operations |
| CPU utilization | To be agreed | Runtime metrics |
| Memory consumption | To be agreed | Runtime metrics |
| Availability | To be agreed | Defined service-level observation |
| Security violations | Zero for prohibited protected operations | Security postcondition checks |

A performance threshold must specify:

- Workload.
- Duration.
- Sample size.
- Network conditions.
- Measurement methodology.
- Allowed exceptions.

Unspecified thresholds must not be treated as passed.

---

# 26. Recovery Time Definitions

The pilot must distinguish three recovery measurements.

## 26.1 Transport recovery time

Time from impairment removal to verified transport availability.

## 26.2 Logical-session recovery time

Time from impairment removal to verification of the required logical-session and authority postconditions.

## 26.3 Application recovery time

Time from impairment removal to successful completion of a defined legitimate application operation.

These metrics must not be used interchangeably.

A restored socket is not proof of logical-session recovery.

---

# 27. Pilot Execution Phases

The recommended pilot consists of six phases.

## Phase A — Preparation

- Scope agreement.
- Artifact selection.
- Threat model agreement.
- Test topology.
- Instrumentation.
- Acceptance thresholds.
- Rollback plan.

## Phase B — Baseline

- Build verification.
- Configuration verification.
- Session identity baseline.
- Authority baseline.
- Application baseline.
- Resource baseline.

## Phase C — Security Validation

- Replay.
- Stale authority.
- Invalid rollback.
- Endpoint manipulation.
- Malformed input.
- Concurrent admission.
- Canonical-state verification.

## Phase D — Resilience Validation

- Endpoint churn.
- Packet loss.
- Blackout.
- High latency.
- Reordering.
- Compound disruption.

## Phase E — Evidence Review

- Test log review.
- Source attribution.
- Integrity verification.
- Failure classification.
- Limitation analysis.
- Remediation assessment.

## Phase F — Acceptance Decision

- Acceptance gate review.
- Residual risk assessment.
- Customer security approval.
- Technical decision.
- Pilot closeout.

Each phase must have an identifiable completion status.

---

# 28. Pilot Roles and Responsibilities

The pilot agreement should assign the following roles.

| Role | Responsibility |
|---|---|
| Protocol Provider | Supply identified evaluation artifact and agreed technical support |
| Customer Technical Owner | Define integration requirements and target environment |
| Customer Security Owner | Approve security scope and risk criteria |
| Test Operator | Execute authorized test procedures |
| Evidence Custodian | Preserve test artifacts and integrity records |
| Independent Reviewer | Assess selected results without assuming author conclusions |
| Acceptance Authority | Issue the formal pilot decision |

A single person may perform multiple roles in a small pilot.

Role consolidation must be disclosed where it affects independence.

---

# 29. Customer Infrastructure Protection

Pilot deployment must not expose production infrastructure to uncontrolled experimental risk.

Required safeguards SHOULD include:

- Isolated test environments.
- Explicit traffic boundaries.
- Controlled credentials.
- Restricted access.
- Resource limits.
- Monitoring.
- Incident escalation.
- Rollback procedures.
- Change approval.
- Data minimization.

Production traffic must not be introduced into the pilot without explicit authorization.

Adversarial testing must remain within approved boundaries.

---

# 30. Data Handling and Confidentiality

The parties must agree on:

- Data classification.
- Log retention.
- Evidence access.
- Credential management.
- Customer data protection.
- Incident notification.
- Artifact storage.
- Evidence deletion or return.
- Publication permissions.

Pilot evidence must not expose confidential customer data without authorization.

Security evidence should be minimized and redacted where possible without destroying its verification value.

---

# 31. Protected Core and Intellectual Property

The public pilot framework does not require disclosure of proprietary implementation algorithms.

The pilot may evaluate:

- Observable protocol behavior.
- Security invariants.
- Test results.
- Artifact integrity.
- Recovery behavior.
- Authority outcomes.
- Application-level performance.

It does not automatically authorize access to:

- Protected Core source code.
- Proprietary authority-resolution mechanisms.
- Private continuity algorithms.
- Internal state-machine implementation.
- Confidential cryptographic material.
- Non-public design documentation.

## 31.1 Access conditions

Any restricted technical access must be governed by a separate written agreement.

## 31.2 Intellectual-property ownership

Ownership, licensing, evaluation rights, derivative-work restrictions and permitted use must be established in the applicable contractual documents.

## 31.3 No implied license

Participation in a pilot does not, by itself, grant rights to proprietary implementation materials beyond those expressly authorized.

## 31.4 Public disclosure

Customer names, results, architectures and pilot conclusions must not be publicly attributed without the necessary permissions.

**PROTECTED CORE DISCLOSURE: NONE**

---

# 32. Pilot Licensing and Artifact Use

The pilot agreement should define:

    LICENSE_TYPE

    AUTHORIZED_ORGANIZATION

    AUTHORIZED_ENVIRONMENT

    EVALUATION_PERIOD

    PERMITTED_USE

    PROHIBITED_USE

    ARTIFACT_ACCESS

    REDISTRIBUTION_POLICY

    TERMINATION_POLICY

    POST_PILOT_RIGHTS

The evaluation framework does not replace a software license or commercial agreement.

Technical acceptance does not automatically grant production licensing rights.

---

# 33. Security Finding Management

Security findings discovered during the pilot must be recorded.

Each finding should contain:

    FINDING_ID

    TITLE

    AFFECTED_INVARIANT

    SOURCE_REVISION

    TEST_ID

    EXPECTED_BEHAVIOR

    OBSERVED_BEHAVIOR

    SECURITY_IMPACT

    EVIDENCE_REFERENCE

    SEVERITY

    STATUS

    REMEDIATION_REFERENCE

    RETEST_RESULT

## 33.1 RED-to-GREEN policy

A correction should follow:

    RED
      |
      v
    ROOT CAUSE ATTRIBUTION
      |
      v
    SCOPED CORRECTION
      |
      v
    GREEN
      |
      v
    REGRESSION
      |
      v
    EVIDENCE VERIFICATION

A correction commit alone is insufficient to close a security finding.

## 33.2 Open findings

Open findings must be reviewed before acceptance.

A mandatory security violation must not be waived silently.

---

# 34. Pilot Stop Conditions

The pilot must define conditions requiring suspension or termination.

Recommended stop conditions include:

- Unauthorized protected execution.
- Demonstrated authority compromise.
- Canonical-state corruption.
- Uncontrolled resource exhaustion.
- Unexpected impact on production systems.
- Credential exposure.
- Evidence integrity failure.
- Test environment escape.
- Unauthorized access.
- Material deviation from the approved test scope.

The test operator must have authority to stop the affected test immediately.

Resumption requires documented approval.

---

# 35. Failure Classification

Failures should be classified by their primary effect.

| Failure class | Description |
|---|---|
| SECURITY_FAILURE | Mandatory security invariant violated |
| CONTINUITY_FAILURE | Required logical-session continuity not preserved |
| AUTHORITY_FAILURE | Unauthorized authority accepted |
| REPLAY_FAILURE | Invalid replay admitted |
| CANONICAL_FAILURE | Unauthorized protected-state mutation |
| APPLICATION_FAILURE | Required application behavior not achieved |
| PERFORMANCE_FAILURE | Agreed performance threshold missed |
| RESOURCE_FAILURE | Agreed resource bound violated |
| EVIDENCE_FAILURE | Required evidence missing or inconsistent |
| HARNESS_FAILURE | Test infrastructure invalidated the result |
| CONFIGURATION_FAILURE | Evaluated configuration deviated from approved baseline |

A harness failure must not automatically be classified as a protocol failure.

A protocol failure must not be dismissed as a harness failure without evidence.

---

# 36. Pilot Evidence Bundle

The recommended evidence package is:

    pilot-evidence/
      PILOT_MANIFEST.md
      SCOPE.md
      ACCEPTANCE_CRITERIA.md
      SOURCE_IDENTITY.txt
      RELEASE_IDENTITY.txt
      BUILD_INFO.txt
      TOPOLOGY.md
      TEST_PLAN.md
      SECURITY_RESULTS.md
      CONTINUITY_RESULTS.md
      PERFORMANCE_RESULTS.md
      APPLICATION_RESULTS.md
      RESOURCE_RESULTS.md
      FINDINGS_REGISTER.md
      RESIDUAL_RISK.md
      FINAL_VERDICT.md
      SHA256SUMS

The actual structure may differ by agreement.

However, the required evidence must remain discoverable and attributable.

---

# 37. Evidence Handover Requirements

Before formal acceptance, the protocol provider and customer should reconcile the evidence inventory.

## Required handover checks

- Artifact identity verified.
- Test inventory complete.
- Mandatory test results available.
- Failed tests retained.
- Inconclusive tests disclosed.
- Source revisions recorded.
- Measurement methods documented.
- Integrity checks completed.
- Open findings listed.
- Residual risks acknowledged.
- Confidential material handled appropriately.

The receiving party should record the date and scope of evidence receipt.

Handover does not imply acceptance of the technical conclusions.

---

# 38. Pilot Acceptance Gate Matrix

The following gates define the proposed acceptance framework.

| Gate | Requirement | Mandatory |
|---|---|---|
| GATE-01 | Approved pilot scope | Yes |
| GATE-02 | Identified evaluated artifact | Yes |
| GATE-03 | Valid baseline | Yes |
| GATE-04 | Security invariant verification | Yes |
| GATE-05 | Independent session identity verification | Yes, when continuity is claimed |
| GATE-06 | Authority integrity | Yes |
| GATE-07 | Replay containment | Yes |
| GATE-08 | Canonical-state integrity | Yes |
| GATE-09 | Application acceptance | As agreed |
| GATE-10 | Performance acceptance | As agreed |
| GATE-11 | Resource acceptance | As agreed |
| GATE-12 | Evidence integrity | Yes |
| GATE-13 | Findings disposition | Yes |
| GATE-14 | Residual risk review | Yes |
| GATE-15 | Formal acceptance decision | Yes |

A mandatory gate that is not satisfied prevents unconditional acceptance.

---

# 39. Acceptance Decision Categories

The final pilot decision must use one of the following categories.

## ACCEPTED

All mandatory acceptance criteria are satisfied.

## CONDITIONALLY ACCEPTED

All non-waivable security requirements are satisfied, but explicitly identified noncritical conditions remain.

Conditional acceptance must include:

- Outstanding conditions.
- Responsible owner.
- Deadline.
- Required retest.
- Approval authority.

## REMEDIATION REQUIRED

One or more required criteria failed and corrective engineering is necessary.

## INCONCLUSIVE

Evidence is insufficient to determine whether the mandatory criteria were satisfied.

## REJECTED

The evaluated implementation does not satisfy the agreed acceptance requirements.

## TERMINATED

The pilot was stopped before qualification could be completed.

A terminated pilot must not be described as successfully accepted.

---

# 40. Non-Waivable Security Conditions

Unless superseded by a stricter agreement, the following must not be waived for an unconditional security acceptance:

1. No demonstrated unauthorized protected execution.
2. No demonstrated unauthorized authority transfer.
3. No demonstrated invalid replay admission.
4. No demonstrated unauthorized canonical-state mutation.
5. No unresolved critical security finding.
6. No missing mandatory artifact identity.
7. No missing mandatory security evidence.
8. No false continuity claim based solely on transport recovery.

A customer may decide to continue limited research after a failed security test.

Such continuation must not be represented as successful security qualification.

---

# 41. Pilot Closeout Report

The final report should contain:

    PILOT_ID

    CUSTOMER_ORGANIZATION

    EVALUATION_CLASS

    EVALUATED_ARTIFACT

    SOURCE_REVISION

    TEST_ENVIRONMENT

    TEST_DURATION

    EXECUTED_TESTS

    PASSED_TESTS

    FAILED_TESTS

    INCONCLUSIVE_TESTS

    BLOCKED_TESTS

    SECURITY_FINDINGS

    CONTINUITY_RESULTS

    APPLICATION_RESULTS

    PERFORMANCE_RESULTS

    RESOURCE_RESULTS

    EVIDENCE_REFERENCES

    RESIDUAL_RISKS

    FINAL_DECISION

    ACCEPTANCE_AUTHORITY

    DECISION_DATE

The report must distinguish author-generated evidence from independently reproduced results.

---

# 42. Recommended Acceptance Record

The following template may be used as the final acceptance record.

    VRP ENTERPRISE PILOT ACCEPTANCE RECORD

    Pilot ID:
    Customer:
    Protocol Provider:
    Evaluated Artifact:
    Artifact SHA-256:
    Evaluation Environment:
    Test Campaign:
    Evidence Bundle:
    Mandatory Gates Passed:
    Mandatory Gates Failed:
    Inconclusive Gates:
    Open Security Findings:
    Accepted Residual Risks:
    Final Decision:
    Conditions:
    Customer Technical Approver:
    Customer Security Approver:
    Protocol Provider Representative:
    Decision Date:

The acceptance record becomes valid only when completed and approved by the authorized parties.

This document does not constitute an executed agreement.

---

# 43. Post-Pilot Progression

Successful completion of a pilot may justify a subsequent evaluation stage.

Possible next steps include:

- Expanded adversarial testing.
- Longer-duration resilience evaluation.
- Additional platform integration.
- Independent security assessment.
- Deployment-specific threat modeling.
- Operational monitoring design.
- Incident-response planning.
- Production readiness assessment.
- Commercial licensing discussions.

No progression is automatic.

A production deployment requires its own operational and security authorization.

---

# 44. Explicit Limitations

This framework does not establish:

- Existing customer adoption.
- Completed enterprise certification.
- Guaranteed production readiness.
- Universal network resilience.
- Complete denial-of-service immunity.
- Absence of undiscovered vulnerabilities.
- Exactly-once application semantics.
- Guaranteed recovery under all failures.
- Telecom standards certification.
- Safety-critical qualification.
- Customer acceptance already obtained.

The results of any future pilot must be evaluated within the documented environment, workload and acceptance criteria.

---

# 45. Related Public Documentation

## Security Assurance Case

    docs/review/VRP_SECURITY_ASSURANCE_CASE.md

## Evidence Traceability Matrix

    docs/review/VRP_EVIDENCE_TRACEABILITY_MATRIX.md

## Adversarial Security Validation Report

    docs/security/VRP_ADVERSARIAL_SECURITY_VALIDATION_REPORT.md

## Threat Model and Trust Boundaries

    docs/security/VRP_THREAT_MODEL_AND_TRUST_BOUNDARIES.md

## Security Findings and Remediation Register

    docs/security/VRP_SECURITY_FINDINGS_AND_REMEDIATION_REGISTER.md

## Autonomous Defense and Fail-Closed Decision Contract

    docs/security/VRP_AUTONOMOUS_DEFENSE_AND_FAIL_CLOSED_DECISION_CONTRACT.md

## Hostile Network Continuity Qualification Matrix

    docs/validation/VRP_HOSTILE_NETWORK_CONTINUITY_QUALIFICATION_MATRIX.md

## Independent Security and Protocol Review Guide

    docs/review/VRP_INDEPENDENT_SECURITY_AND_PROTOCOL_REVIEW_GUIDE.md

## Public Validation Kit

https://github.com/Endless33/vrp-validation-kit

## VRP Project

https://github.com/Endless33/Veil-Routing-Protocol

All referenced files and executable artifacts must be verified against their actual published revisions.

---

# 46. Final Engineering Statement

Enterprise protocol evaluation requires more than a successful demonstration.

A credible pilot must answer:

- Which artifact was evaluated?
- Which security invariants were tested?
- Which network conditions were reproduced?
- Was the original logical session preserved?
- Was authority integrity maintained?
- Were invalid replay operations rejected?
- Was canonical state protected?
- Did legitimate application operations succeed?
- Were performance targets satisfied?
- Can the evidence be independently examined?
- What remains unverified?

The VRP enterprise pilot framework is based on:

**SESSION != TRANSPORT**

**RECONNECTION != VERIFIED CONTINUITY**

**REACHABILITY != AUTHORITY**

**REPLAY != NEW EXECUTION**

**RECOVERY != SECURITY RESET**

**APPLICATION DELIVERY != APPLICATION COMMIT**

**TEST EXECUTION != TEST ACCEPTANCE**

**PILOT ACCEPTANCE != PRODUCTION AUTHORIZATION**

**SECURITY CLAIM != VERIFIED EVIDENCE**

The enterprise objective is:

**QUALIFY CONTINUITY WITHOUT COMPROMISING AUTHORITY, CANONICAL STATE OR EXECUTION INTEGRITY.**

The acceptance discipline is:

**DEFINE THE CONTRACT.**

**IDENTIFY THE ARTIFACT.**

**EXECUTE THE TESTS.**

**VERIFY THE SESSION.**

**VERIFY THE AUTHORITY.**

**PRESERVE THE EVIDENCE.**

**RECORD THE DECISION.**

---

**END OF DOCUMENT**

**Veil Routing Protocol — VRP**

**Author:** Vitalijus Riabovas

© 2026 Vitalijus Riabovas. All rights reserved.

This document defines a proposed enterprise pilot evaluation framework. It does not constitute a commercial offer, executed agreement, certification, production authorization or disclosure of proprietary implementation mechanisms. Applicable repository license terms and separately executed agreements govern use.
