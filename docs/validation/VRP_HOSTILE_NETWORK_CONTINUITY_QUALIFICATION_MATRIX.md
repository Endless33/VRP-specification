
# VRP CONTINUITY UNDER HOSTILE NETWORK CONDITIONS

## Qualification Matrix, Independent Session Identity Verification and Compound Failure Assessment

**Document ID:** VRP-HNCQM-007  
**Version:** 1.0  
**Date:** 2026-11-12  
**Status:** Public Qualification Specification — Proposed  
**Classification:** PUBLIC — NON-PROPRIETARY  
**Protocol:** Veil Routing Protocol (VRP)  
**Author:** Vitalijus Riabovas  
**Project:** VRP / Jumping VPN

---

# 1. Executive Summary

Veil Routing Protocol (VRP) is a continuity-first networking architecture based on the principle:

**SESSION != TRANSPORT**

The central architectural proposition is that logical-session identity should not be determined exclusively by the lifetime of an individual transport connection.

Network paths may fail, change or become temporarily unavailable.

Such events must not automatically imply that the logical session has been destroyed.

However, successful reconnection does not establish session continuity.

Likewise, successful application delivery does not independently establish authority integrity.

This document defines a public qualification framework for evaluating VRP under hostile and unstable network conditions.

The qualification program addresses:

- Rapid endpoint churn.
- Packet loss.
- Network blackout.
- High round-trip latency.
- Jitter and packet reordering.
- Replay pressure.
- Conflicting authority.
- Concurrent disruption.
- Compound network failures.
- Logical-session identity verification.
- Application-level progress.
- Recovery-time measurement.
- Resource consumption.
- Evidence integrity.

The central qualification requirement is:

**TRANSPORT RECOVERY MUST NOT BE MISTAKEN FOR VERIFIED LOGICAL-SESSION CONTINUITY.**

This document defines proposed test procedures and acceptance criteria.

It does not claim that every listed scenario has already been executed or passed.

---

# 2. Qualification Objectives

The qualification program evaluates five independent properties.

## 2.1 Logical-session continuity

Determine whether the original logical session remains valid across a supported transport disruption.

## 2.2 Authority integrity

Determine whether unauthorized, stale or conflicting authority can modify protected state during disruption or recovery.

## 2.3 Replay containment

Determine whether invalid historical or duplicated operations remain rejected.

## 2.4 Application progress

Determine whether legitimate application operations can continue or resume after disruption.

## 2.5 Operational behavior

Measure recovery time, delivery outcomes, resource consumption and failure characteristics.

These properties must be reported separately.

A PASS in one category must not be interpreted as a PASS in every category.

---

# 3. Normative Language

The terms MUST, MUST NOT, SHOULD, SHOULD NOT and MAY are used as described in RFC 2119 and RFC 8174 when capitalized.

These terms establish qualification requirements.

They do not imply that a particular runtime revision has already demonstrated conformance.

---

# 4. Foundational Qualification Principles

The following distinctions are mandatory.

**SESSION != TRANSPORT**

Logical-session identity is not defined solely by a socket, endpoint or network path.

**RECONNECTION != CONTINUITY**

Restored connectivity does not independently prove preservation of the original logical session.

**PROCESS SURVIVAL != SESSION SURVIVAL**

An unchanged process identifier is not a session-identity oracle.

**REACHABILITY != AUTHORITY**

A reachable endpoint does not automatically possess execution authority.

**RECOVERY != SECURITY RESET**

Recovery must not silently reactivate invalid authority or bypass replay protection.

**PACKET DELIVERY != APPLICATION COMMIT**

Receiving data does not necessarily establish successful application-level execution.

**TEST PASS != UNIVERSAL GUARANTEE**

A passing result applies only to its documented conditions and acceptance criteria.

---

# 5. Qualification Scope

This document defines tests for controlled laboratory environments.

The intended evaluation surfaces include:

- Protocol runtime.
- Network transport.
- Operating-system networking.
- Session identity.
- Authority state.
- Canonical state.
- Application delivery.
- Recovery behavior.

The evaluation may use:

- Linux virtual machines.
- Controlled network impairment.
- Isolated test networks.
- Instrumented application endpoints.
- Independent verification processes.
- Adversarial traffic generators.

Testing MUST be conducted only against systems and infrastructure for which the evaluator has authorization.

---

# 6. Test Environment Identity

Every qualification run MUST identify the evaluated environment.

Required fields:

    CAMPAIGN_ID
    TEST_ID
    SOURCE_REPOSITORY
    SOURCE_COMMIT
    SOURCE_TREE
    BUILD_ID
    RUNTIME_VERSION
    OPERATING_SYSTEM
    KERNEL_VERSION
    NETWORK_TOPOLOGY
    IMPAIRMENT_TOOL
    IMPAIRMENT_TOOL_VERSION
    APPLICATION_TEST_VERSION
    SESSION_ORACLE_VERSION
    EXECUTION_TIMESTAMP
    EVIDENCE_DIRECTORY

Where a field is unavailable, the limitation MUST be explicitly recorded.

A result without attributable source identity MUST NOT be presented as release qualification.

---

# 7. Independent Session Identity Oracle

The most important requirement of this qualification program is independent logical-session identity verification.

A test MUST NOT infer logical-session continuity solely from:

- Process survival.
- Socket survival.
- UDP reconnection.
- Interface persistence.
- Tunnel restoration.
- Endpoint replacement.
- Successful packet delivery.
- Application process survival.

These observations may provide supporting evidence.

They are not sufficient proof of logical-session identity.

## 7.1 Required oracle properties

The session-identity oracle MUST:

1. Observe the relevant logical-session identity.
2. Distinguish session identity from transport identity.
3. Detect unexpected session replacement.
4. Provide attributable observations.
5. Operate independently of the transport-recovery success signal.
6. Preserve sufficient evidence for external review.

Independence does not necessarily require a separate physical machine.

It requires that the identity conclusion is not circularly derived from the transport event being evaluated.

## 7.2 Required observation tuple

    LOGICAL_SESSION_IDENTITY
    TRANSPORT_IDENTITY
    AUTHORITY_STATE
    CANONICAL_STATE
    APPLICATION_PROGRESS
    OBSERVATION_TIMESTAMP

## 7.3 Required baseline

Before impairment:

    SESSION_IDENTITY = S0

    AUTHORITY_STATE = A0

    CANONICAL_STATE = C0

    TRANSPORT_IDENTITY = T0

The identifiers above are symbolic observation labels, not prescribed implementation formats.

## 7.4 Required postcondition

After supported transport recovery:

    SESSION_IDENTITY = S0

The evaluator must additionally verify that the resulting authority and canonical state satisfy the applicable protocol contract.

A transport replacement may produce:

    TRANSPORT_IDENTITY = T1

without implying a new logical session.

## 7.5 Oracle failure

If the oracle cannot establish identity continuity, the result MUST be classified as:

    CONTINUITY_VERDICT = INCONCLUSIVE

Successful reconnection MUST NOT override this classification.

---

# 8. Qualification Verdict Model

Each test MUST report separate verdicts for:

    CONTINUITY_VERDICT
    AUTHORITY_VERDICT
    REPLAY_VERDICT
    APPLICATION_VERDICT
    RESOURCE_VERDICT
    EVIDENCE_VERDICT

Permitted verdicts:

| Verdict | Meaning |
|---|---|
| PASS | All applicable criteria satisfied |
| FAIL | At least one required criterion violated |
| INCONCLUSIVE | Evidence insufficient |
| NOT RUN | Scenario not executed |
| BLOCKED | Required prerequisite unavailable |
| NOT APPLICABLE | Property outside declared test scope |

An overall PASS requires every mandatory dimension to pass.

A security failure MUST NOT be concealed by successful transport recovery.

---

# 9. Qualification Scenario Matrix

The following scenarios establish the baseline qualification campaign.

| Test ID | Scenario | Primary objective |
|---|---|---|
| VRP-HNC-001 | Baseline without impairment | Establish reference behavior |
| VRP-HNC-002 | Endpoint churn every 2 seconds | Verify continuity across path replacement |
| VRP-HNC-003 | Packet loss 30% | Measure moderate-loss behavior |
| VRP-HNC-004 | Packet loss 50% | Measure severe-loss behavior |
| VRP-HNC-005 | Packet loss 80% | Evaluate extreme-loss behavior |
| VRP-HNC-006 | Packet loss 90% | Establish degradation and recovery limits |
| VRP-HNC-007 | Blackout 10 seconds | Verify short-outage recovery |
| VRP-HNC-008 | Blackout 30 seconds | Verify extended-outage behavior |
| VRP-HNC-009 | Blackout 60 seconds | Evaluate prolonged disruption |
| VRP-HNC-010 | RTT 300–900 ms | Evaluate high-latency behavior |
| VRP-HNC-011 | Jitter and reordering | Evaluate delayed and reordered delivery |
| VRP-HNC-012 | Replay during migration | Verify replay containment |
| VRP-HNC-013 | Competing authority recovery | Verify authority integrity |
| VRP-HNC-014 | Churn + 80% loss + blackout + replay | Evaluate compound failure |
| VRP-HNC-015 | Post-recovery application verification | Verify legitimate execution progress |

All scenarios are proposed qualification cases.

No PASS status is implied by their inclusion.

---

# 10. VRP-HNC-001 — Baseline Reference Test

## Objective

Establish the system's behavior before introducing impairment.

## Preconditions

- Identified runtime revision.
- Controlled network topology.
- Valid logical session.
- Authorized execution context.
- Operational session-identity oracle.
- Application test workload.

## Required measurements

- Logical-session identity.
- Transport identity.
- Authority state.
- Canonical state.
- Application delivery rate.
- Application latency.
- CPU usage.
- Memory usage.

## Acceptance criteria

The baseline MUST demonstrate valid application progress and consistent session-identity observations.

A failed baseline invalidates subsequent comparisons until the underlying problem is resolved.

---

# 11. VRP-HNC-002 — Endpoint Churn Every 2 Seconds

## Objective

Determine whether the logical session remains valid while its network endpoint changes repeatedly.

## Test configuration

    ENDPOINT_CHURN_INTERVAL = 2 SECONDS

    CHURN_MODE = CONTROLLED

    SESSION_ORACLE = REQUIRED

    AUTHORITY_OBSERVATION = REQUIRED

The total test duration and number of transitions MUST be recorded before execution.

## Required observations

- Original logical-session identity.
- Each endpoint transition.
- Transport identity after each transition.
- Authority state.
- Canonical-state integrity.
- Application progress.
- Failed migration attempts.
- Recovery duration per transition.

## Acceptance criteria

For every transition claimed to preserve the session:

    SESSION_IDENTITY_AFTER = SESSION_IDENTITY_BEFORE

    UNAUTHORIZED_AUTHORITY_TRANSFER = FALSE

    UNAUTHORIZED_CANONICAL_MUTATION = FALSE

The test MUST distinguish authorized migration from rejected endpoint adoption.

## Failure conditions

- Unexpected logical-session replacement.
- Unauthorized endpoint adoption.
- Unauthorized authority transfer.
- Canonical-state corruption.
- Incorrectly reported continuity.

A temporary delivery interruption is not automatically a security failure.

Its effect on availability qualification must be reported separately.

---

# 12. VRP-HNC-003 — Packet Loss 30%

## Objective

Evaluate continuity and application behavior under moderate packet loss.

## Configuration

    PACKET_LOSS = 30%

    LOSS_MODEL = DOCUMENTED

    DURATION = RECORDED

The evaluator MUST document whether loss is independent, burst-based or generated using another impairment model.

## Required measurements

- Application delivery.
- Duplicate delivery.
- Logical-session identity.
- Authority integrity.
- Replay decisions.
- Recovery events.
- Resource consumption.

## Acceptance criteria

Security invariants MUST remain satisfied.

Application-level acceptance thresholds MUST be defined before execution.

No universal throughput or recovery-time guarantee is implied.

---

# 13. VRP-HNC-004 — Packet Loss 50%

## Objective

Evaluate behavior under severe packet loss.

## Configuration

    PACKET_LOSS = 50%

    LOSS_MODEL = DOCUMENTED

## Required verification

- Independent session identity.
- Authority integrity.
- Replay containment.
- Application delivery.
- Recovery timing.
- Resource consumption.

## Acceptance criteria

The runtime MUST NOT bypass security requirements to compensate for packet loss.

Any continuity PASS requires independent identity verification.

Availability results must be reported separately.

---

# 14. VRP-HNC-005 — Packet Loss 80%

## Objective

Evaluate the system under extreme packet loss.

## Configuration

    PACKET_LOSS = 80%

    LOSS_MODEL = DOCUMENTED

## Primary questions

1. Does the original logical session remain identifiable?
2. Does authority remain valid?
3. Are invalid replay attempts rejected?
4. Does application progress continue or resume?
5. Does the runtime remain within documented resource limits?
6. Is recovery time measurable?

## Acceptance criteria

Security invariants MUST remain satisfied.

A continuity PASS MUST NOT be inferred solely from eventual packet delivery.

The evaluator MUST explicitly distinguish:

    SESSION_PRESERVED

    SESSION_TERMINATED_SAFELY

    SESSION_IDENTITY_UNVERIFIED

Safe termination is not equivalent to successful continuity.

---

# 15. VRP-HNC-006 — Packet Loss 90%

## Objective

Establish observed behavior near severe operational degradation.

## Configuration

    PACKET_LOSS = 90%

    LOSS_MODEL = DOCUMENTED

## Required observations

- Session identity.
- Authority state.
- Replay decisions.
- Application progress.
- Recovery attempts.
- Resource consumption.
- Terminal state where applicable.

## Acceptance criteria

Security requirements MUST remain enforced.

A system that safely rejects invalid operations while failing to deliver application traffic may satisfy a security-integrity criterion but fail an availability criterion.

These outcomes MUST NOT be combined into a misleading overall PASS.

---

# 16. VRP-HNC-007 — Blackout 10 Seconds

## Objective

Evaluate recovery after a short transport blackout.

## Configuration

    BLACKOUT_DURATION = 10 SECONDS

    SESSION_ORACLE = REQUIRED

## Required timeline

    T0 = BASELINE_OBSERVATION

    T1 = BLACKOUT_START

    T2 = BLACKOUT_END

    T3 = TRANSPORT_RESTORED

    T4 = APPLICATION_PROGRESS_RESTORED

The evaluator MUST record the actual observed times rather than assume they equal the configured impairment duration.

## Required postconditions

- Logical-session identity verified.
- Authority integrity preserved.
- Invalid replay rejected.
- Canonical state consistent.
- Application progress measured.

## Recovery metrics

    TRANSPORT_RECOVERY_TIME = T3 - T2

    APPLICATION_RECOVERY_TIME = T4 - T2

The two metrics MUST remain distinct.

---

# 17. VRP-HNC-008 — Blackout 30 Seconds

## Objective

Evaluate behavior following an extended interruption.

## Configuration

    BLACKOUT_DURATION = 30 SECONDS

## Required observations

- Logical-session identity before and after blackout.
- Authority state.
- Canonical-state integrity.
- Replay handling.
- Transport recovery.
- Application progress.
- Recovery time.

## Acceptance criteria

A recovered connection MUST NOT be classified as preserved session continuity without independent identity verification.

The result MUST identify whether the session:

- Remained valid.
- Was safely terminated.
- Was replaced.
- Could not be conclusively classified.

---

# 18. VRP-HNC-009 — Blackout 60 Seconds

## Objective

Evaluate prolonged transport unavailability.

## Configuration

    BLACKOUT_DURATION = 60 SECONDS

## Required verification

- Session identity.
- Authority integrity.
- Replay containment.
- Canonical-state integrity.
- Application progress.
- Recovery timing.
- Resource consumption.

## Acceptance criteria

The implementation MUST NOT restore invalid authority or accept prohibited historical execution merely because the blackout was prolonged.

A failure to preserve continuity MUST be reported accurately.

A safe failure may preserve security integrity without satisfying the continuity objective.

---

# 19. VRP-HNC-010 — High-Latency Network

## Objective

Evaluate continuity under satellite-like round-trip latency.

## Configuration

    RTT_MIN = 300 MS

    RTT_MAX = 900 MS

    LATENCY_PROFILE = DOCUMENTED

The test MUST identify whether latency is fixed, variable or applied according to a recorded distribution.

## Required measurements

- Actual RTT distribution.
- Application operation latency.
- Delivery success.
- Session identity.
- Authority state.
- Replay decisions.
- Recovery behavior.

## Acceptance criteria

Delayed packets MUST remain subject to the applicable security contract.

High latency MUST NOT grant authority to stale or otherwise invalid operations.

---

# 20. VRP-HNC-011 — Jitter and Reordering

## Objective

Evaluate protocol behavior when packets arrive with variable delay or out of order.

## Configuration

    JITTER = ENABLED

    REORDERING = ENABLED

    PARAMETERS = RECORDED

## Required verification

- Reordering profile.
- Delivery order.
- Replay rejection.
- Authority integrity.
- Canonical-state consistency.
- Application progress.

## Acceptance criteria

Reordering MUST NOT result in unauthorized protected execution.

Valid delayed traffic must be evaluated according to the applicable protocol eligibility contract.

---

# 21. VRP-HNC-012 — Replay During Migration

## Objective

Determine whether replay containment remains effective while the transport path changes.

## Threat scenario

Previously observed traffic is resubmitted during an authorized or attempted migration.

## Required observations

- Original session identity.
- Migration event.
- Replay input.
- Admission decision.
- Authority state.
- Canonical-state postcondition.
- Application progress.

## Acceptance criteria

    INVALID_REPLAY_ACCEPTED = FALSE

    UNAUTHORIZED_DUPLICATE_COMMIT = FALSE

    UNAUTHORIZED_AUTHORITY_TRANSFER = FALSE

The test MUST distinguish rejected replay from legitimate post-migration traffic.

---

# 22. VRP-HNC-013 — Competing Authority Recovery

## Objective

Evaluate authority integrity when conflicting authority claims arise during recovery.

## Required observations

- Initial authority state.
- Competing claims.
- Admission outcomes.
- Final canonical authority.
- Session identity.
- Canonical-state integrity.
- Repeatability.

## Acceptance criteria

Unauthorized or obsolete authority MUST NOT obtain protected execution rights.

The result MUST satisfy the applicable deterministic authority contract.

The test MUST NOT infer authority correctness solely from restored connectivity.

---

# 23. VRP-HNC-014 — Compound Failure Qualification

## Objective

Evaluate VRP under interacting network and security disruptions.

This scenario combines multiple impairment classes.

## Proposed configuration

    ENDPOINT_CHURN_INTERVAL = 2 SECONDS

    PACKET_LOSS = 80%

    BLACKOUT = ENABLED

    REPLAY_PRESSURE = ENABLED

    SESSION_ORACLE = REQUIRED

    AUTHORITY_OBSERVATION = REQUIRED

The blackout duration, replay intensity, total test duration and impairment ordering MUST be fixed in the execution manifest.

## 23.1 Required observations

- Logical-session identity.
- Endpoint transitions.
- Actual packet-loss rate.
- Blackout start and end.
- Replay submission count.
- Replay rejection count.
- Authority state.
- Canonical-state integrity.
- Application delivery.
- Recovery time.
- Resource consumption.

## 23.2 Required security postconditions

    UNAUTHORIZED_REPLAY_ACCEPTANCE = FALSE

    UNAUTHORIZED_AUTHORITY_TRANSFER = FALSE

    UNAUTHORIZED_CANONICAL_MUTATION = FALSE

    INVALID_EPOCH_ROLLBACK = FALSE

## 23.3 Continuity postcondition

If the scenario is within the implementation's declared continuity-support envelope:

    SESSION_IDENTITY_AFTER = SESSION_IDENTITY_BEFORE

If continuity cannot be established, the result MUST be classified as FAIL or INCONCLUSIVE according to the observed evidence and predefined acceptance criteria.

## 23.4 Qualification limitation

Passing this compound profile does not establish success for every possible combination of impairment parameters.

The exact profile MUST remain attached to the result.

---

# 24. VRP-HNC-015 — Application Progress After Recovery

## Objective

Verify that legitimate application-level operations can resume after supported transport disruption.

## Required observations

- Application operation identifier.
- Submission timestamp.
- Delivery timestamp.
- Application acknowledgement where applicable.
- Commit result where applicable.
- Duplicate execution count.
- Logical-session identity.
- Authority state.

## Important distinction

Packet delivery does not automatically prove application commit.

Application-level exactly-once behavior requires a separately defined execution contract.

## Acceptance criteria

The evaluator MUST establish the applicable application-level success condition before running the test.

The reported result MUST distinguish:

- Transport delivery.
- Application receipt.
- Application acknowledgement.
- Application commit.

---

# 25. Recovery Time Measurement

Recovery time MUST be reported using explicit start and end events.

## 25.1 Transport recovery

The time from impairment removal to verified transport availability.

## 25.2 Session recovery

The time from impairment removal to the point at which the required logical-session and authority postconditions are verified.

## 25.3 Application recovery

The time from impairment removal to successful completion of a defined legitimate application operation.

These metrics may differ.

They MUST NOT be reported as interchangeable.

## 25.4 Measurement requirements

- Monotonic timing where possible.
- Explicit event definitions.
- Recorded measurement resolution.
- Clock synchronization assumptions.
- Repeated measurements.
- Distribution statistics.

Recommended reporting includes:

    MINIMUM

    MEDIAN

    P95

    P99

    MAXIMUM

Sample size MUST be disclosed.

Percentiles MUST NOT be presented as meaningful when the sample size or measurement methodology is insufficient.

---

# 26. Application Delivery Metrics

The qualification report SHOULD include:

    OPERATIONS_SUBMITTED

    OPERATIONS_DELIVERED

    OPERATIONS_ACKNOWLEDGED

    OPERATIONS_COMMITTED

    DUPLICATE_OPERATIONS

    FAILED_OPERATIONS

    TIMED_OUT_OPERATIONS

Each metric MUST have an explicit definition.

The evaluator SHOULD distinguish network loss from application-level rejection.

A high delivery rate does not independently establish authority integrity.

---

# 27. Resource Consumption

Resource measurements SHOULD include:

- CPU utilization.
- Resident memory.
- Queue or buffer utilization where available.
- Open file descriptors where relevant.
- Process restarts.
- Runtime crashes.
- Sustained resource growth.
- Recovery resource peaks.

Resource limits MUST be stated before a bounded-resource PASS is claimed.

Functional correctness under a short test does not establish indefinite resource stability.

---

# 28. Repetition and Statistical Confidence

A single successful execution provides limited assurance.

Qualification SHOULD include repeated trials.

The execution manifest MUST specify:

    TRIAL_COUNT

    RANDOM_SEED_POLICY

    IMPAIRMENT_PARAMETERS

    TIMEOUT_POLICY

    ACCEPTANCE_THRESHOLDS

    RESULT_AGGREGATION_POLICY

Failed trials MUST NOT be silently excluded.

Results SHOULD report both individual failures and aggregate statistics.

Repeated success increases confidence but does not constitute a formal proof of correctness.

---

# 29. Failure Classification

Failures SHOULD be classified according to the violated property.

| Failure class | Description |
|---|---|
| IDENTITY_FAILURE | Unexpected logical-session replacement |
| AUTHORITY_FAILURE | Unauthorized authority accepted |
| REPLAY_FAILURE | Invalid replay admitted |
| CANONICAL_FAILURE | Unauthorized protected-state mutation |
| APPLICATION_FAILURE | Required application progress not achieved |
| RECOVERY_FAILURE | Recovery contract not satisfied |
| RESOURCE_FAILURE | Documented resource bound violated |
| EVIDENCE_FAILURE | Required observations unavailable or inconsistent |
| HARNESS_FAILURE | Test infrastructure invalidated the run |

A harness failure MUST NOT automatically be classified as a protocol security failure.

Likewise, a protocol failure MUST NOT be dismissed as a harness problem without evidence.

---

# 30. Evidence Bundle Requirements

Each qualification run SHOULD produce:

    evidence/
      hostile-network/
        <campaign-id>/
          MANIFEST.md
          SOURCE_IDENTITY.txt
          BUILD_INFO.txt
          TOPOLOGY.md
          IMPAIRMENT_CONFIGURATION.json
          SESSION_ORACLE.log
          AUTHORITY_OBSERVATIONS.log
          CANONICAL_STATE.log
          APPLICATION_RESULTS.json
          RESOURCE_METRICS.csv
          EXECUTION.log
          VERDICT.md
          SHA256SUMS

The evidence directory MUST be finalized before checksums are generated.

Public evidence MUST NOT expose protected algorithms, secret keys, credentials or confidential infrastructure details.

---

# 31. Qualification Manifest

Every run SHOULD contain a manifest with the following structure:

    CAMPAIGN_ID:
    TEST_ID:
    SOURCE_REPOSITORY:
    SOURCE_COMMIT:
    SOURCE_TREE:
    BUILD_ID:
    OPERATING_SYSTEM:
    KERNEL_VERSION:
    TEST_TOPOLOGY:
    IMPAIRMENT_TOOL:
    IMPAIRMENT_PROFILE:
    TRIAL_COUNT:
    SESSION_ORACLE:
    APPLICATION_WORKLOAD:
    ACCEPTANCE_THRESHOLDS:
    START_TIMESTAMP:
    END_TIMESTAMP:
    EVIDENCE_REFERENCE:
    VERDICT:

All fields MUST reflect actual execution records.

Unknown values MUST NOT be fabricated.

---

# 32. Qualification Acceptance Gates

A formal qualification PASS requires all applicable gates.

## GATE 1 — Artifact Identity

The evaluated implementation is identifiable.

## GATE 2 — Valid Baseline

The pre-impairment state satisfies the required contract.

## GATE 3 — Applied Impairment

The intended impairment was demonstrably applied.

## GATE 4 — Independent Identity

Logical-session identity is independently verified where continuity is claimed.

## GATE 5 — Authority Integrity

Unauthorized authority was not accepted.

## GATE 6 — Replay Containment

Invalid replay did not create unauthorized execution.

## GATE 7 — Canonical Integrity

Protected state remained consistent with the contract.

## GATE 8 — Application Behavior

Required application progress was measured and satisfied the predefined acceptance criteria.

## GATE 9 — Resource Behavior

Applicable resource bounds were evaluated.

## GATE 10 — Evidence Integrity

The result is attributable and internally consistent.

## GATE 11 — Scope Declaration

Limitations and exclusions are explicit.

Missing mandatory evidence MUST prevent an unqualified PASS verdict.

---

# 33. Qualification Coverage Accounting

Coverage MUST be reported separately for:

- Endpoint churn.
- Packet loss.
- Blackout.
- High latency.
- Jitter.
- Reordering.
- Replay.
- Authority contention.
- Compound failures.
- Application progress.

Suggested coverage metric:

    EXECUTED_SCENARIOS / PLANNED_SCENARIOS

Passing coverage:

    PASSED_SCENARIOS / EXECUTED_SCENARIOS

Independent reproduction coverage:

    INDEPENDENTLY_REPRODUCED_SCENARIOS
    /
    ELIGIBLE_SCENARIOS

A scenario count is not equivalent to complete threat coverage.

The coverage denominator and exclusions MUST be disclosed.

---

# 34. Public Evidence Status

This document is a qualification specification.

It is not a completed campaign report.

The proposed scenario matrix does not assert that all scenarios have passed.

Existing public VRP validation artifacts may provide evidence for selected protocol invariants.

However, a public reference-model PASS does not automatically establish integrated runtime continuity.

For formal publication, each runtime result requires:

- Identified source revision.
- Reproducible test configuration.
- Recorded impairment.
- Independent session-identity evidence.
- Authority and canonical-state observations.
- Application-level measurements.
- Final verdict.

Until such evidence is attached, the scenario remains:

    QUALIFICATION EVIDENCE PENDING

---

# 35. Independent Reviewer Procedure

An independent evaluator SHOULD:

1. Select a qualification scenario.
2. Verify the implementation identity.
3. Inspect the test configuration.
4. Establish the baseline.
5. Verify the independent session oracle.
6. Apply the documented impairment.
7. Capture transport behavior.
8. Verify logical-session identity.
9. Verify authority integrity.
10. Verify replay containment.
11. Verify canonical-state integrity.
12. Measure application progress.
13. Measure recovery time.
14. Inspect resource consumption.
15. Verify evidence integrity.
16. Publish a scoped verdict.

The evaluator MUST distinguish independently reproduced observations from results reported by the implementation author.

---

# 36. Residual Risks and Limitations

## 36.1 Incomplete impairment coverage

Finite test profiles cannot represent every possible network failure.

## 36.2 Environment dependence

Results from virtualized Linux environments may not transfer directly to every production platform.

## 36.3 Statistical limitations

A small number of successful trials does not establish a reliable tail-latency guarantee.

## 36.4 Application semantics

Protocol-level continuity does not automatically establish exactly-once application execution.

## 36.5 Resource exhaustion

Passing a functional recovery test does not establish resistance to unlimited denial-of-service pressure.

## 36.6 Session-oracle limitations

An oracle that depends on the same assumptions as the transport recovery mechanism may produce circular evidence.

## 36.7 Operational qualification

Laboratory validation does not automatically establish readiness for telecom core networks, industrial control systems or safety-critical infrastructure.

---

# 37. Protected Core Disclosure Boundary

This document intentionally exposes:

- Qualification scenarios.
- Network impairment parameters.
- Security invariants.
- Required observations.
- Acceptance criteria.
- Evidence requirements.
- Failure classifications.
- Assurance limitations.

It intentionally excludes:

- Proprietary session-continuity algorithms.
- Internal authority-resolution mechanisms.
- Private state-machine implementation.
- Protected recovery logic.
- Confidential packet-processing internals.
- Secret cryptographic material.
- Private deployment configuration.
- Non-public runtime source code.

**PROTECTED CORE DISCLOSURE: NONE**

Independent qualification is based on observable contracts and attributable evidence.

---

# 38. Related Documentation

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

## Public Validation Kit

https://github.com/Endless33/vrp-validation-kit

## VRP Project

https://github.com/Endless33/Veil-Routing-Protocol

---

# 39. Final Engineering Statement

A continuity-first protocol must be evaluated under conditions where transport stability can no longer be assumed.

The relevant engineering questions are not limited to whether connectivity returns.

They include:

- Did the original logical session survive?
- Did authority remain valid?
- Were invalid replay attempts rejected?
- Was canonical history preserved?
- Did legitimate application execution resume?
- How long did recovery take?
- What resources were consumed?
- Can the result be independently reproduced?

The VRP qualification framework is based on:

**SESSION != TRANSPORT**

**RECONNECTION != VERIFIED CONTINUITY**

**REACHABILITY != AUTHORITY**

**REPLAY != NEW EXECUTION**

**RECOVERY != SECURITY RESET**

**PACKET DELIVERY != APPLICATION COMMIT**

**SECURITY CLAIM != VERIFIED EVIDENCE**

The ultimate qualification objective is:

**PRESERVE LOGICAL-SESSION CONTINUITY WITHOUT COMPROMISING AUTHORITY, CANONICAL STATE OR EXECUTION INTEGRITY.**

The required engineering discipline is:

**MEASURE THE SESSION.**

**VERIFY THE AUTHORITY.**

**PRESERVE THE HISTORY.**

**RECORD THE EVIDENCE.**

---

**END OF DOCUMENT**

**Veil Routing Protocol — VRP**

**Author:** Vitalijus Riabovas

© 2026 Vitalijus Riabovas. All rights reserved.

This public document defines qualification procedures, expected behavior and evidence requirements. It does not disclose proprietary implementation mechanisms or grant access to Protected Core materials. Applicable repository license terms govern reuse.
