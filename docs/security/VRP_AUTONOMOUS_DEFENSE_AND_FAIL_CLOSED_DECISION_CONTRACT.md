
# VRP AUTONOMOUS DEFENSE & FAIL-CLOSED DECISION CONTRACT

## Veil Routing Protocol (VRP)
### Autonomous Security Decisions, Admission Integrity and Continuity Preservation

**Document ID:** VRP-ADFCDC-006  
**Version:** 1.0  
**Date:** 2026-11-12  
**Status:** Public Normative Security Contract — Proposed  
**Classification:** PUBLIC — NON-PROPRIETARY  
**Author:** Vitalijus Riabovas  
**Project:** Veil Routing Protocol / Jumping VPN

---

# 1. Executive Summary

The Veil Routing Protocol (VRP) is an experimental continuity-first networking architecture built around the principle:

**SESSION != TRANSPORT**

A logical session is not defined exclusively by the lifetime of an individual network connection, socket, endpoint or transport path.

This separation introduces a critical security responsibility.

When the network becomes unstable, hostile or unpredictable, the protocol must distinguish between legitimate continuity operations and unauthorized attempts to influence protected state.

A continuity-first protocol must not preserve connectivity by sacrificing authority integrity.

It must not accept unknown input merely to maintain availability.

It must not allow replayed operations to create new canonical execution.

It must not interpret a changed endpoint as proof of legitimate authority.

It must not reset security boundaries simply because recovery is required.

This document defines the proposed public contract for autonomous defensive decisions within VRP.

Its central principle is:

**AUTONOMOUS RECOVERY MUST NEVER BECOME AUTONOMOUS AUTHORITY ESCALATION.**

The objective is to establish observable and independently testable requirements for security decisions under adversarial conditions.

This document describes required behavior, not proprietary decision algorithms.

It does not claim that every requirement has already been verified in an integrated production runtime.

---

# 2. Normative Language

The terms MUST, MUST NOT, REQUIRED, SHOULD, SHOULD NOT and MAY are used as described by RFC 2119 and RFC 8174 when capitalized.

**MUST / MUST NOT**

Mandatory requirements for an implementation claiming conformance to this contract.

**SHOULD / SHOULD NOT**

Strong recommendations that may be departed from only with documented justification and evaluated consequences.

**MAY**

Optional behavior permitted by this contract.

A normative requirement is not equivalent to a completed implementation or verified test result.

Conformance must be established through separately attributable evidence.

---

# 3. Scope

This contract addresses autonomous security behavior during:

- Unknown network input.
- Malformed packet reception.
- Invalid admission attempts.
- Replay and duplicate traffic.
- Endpoint changes.
- Transport migration.
- Stale authority.
- Conflicting authority.
- Invalid epoch rollback.
- Temporary network blackout.
- Packet loss and reordering.
- Concurrent admission.
- Resource pressure.
- Compound network failures.
- Recovery after disruption.

The contract applies to observable protocol behavior and protected-state outcomes.

It does not prescribe internal implementation structure.

---

# 4. Foundational Security Principles

## 4.1 Session continuity

**VRP-ADF-001**

A conforming implementation MUST distinguish logical-session identity from transport identity.

Transport replacement MUST NOT, by itself, authorize a new logical session identity.

## 4.2 Authority separation

**VRP-ADF-002**

Transport reachability MUST NOT be treated as proof of execution authority.

## 4.3 Fail-closed admission

**VRP-ADF-003**

Input that cannot satisfy the applicable admission requirements MUST NOT obtain protected execution authority.

## 4.4 Canonical-state integrity

**VRP-ADF-004**

Rejected operations MUST NOT produce unauthorized persistent changes to protected canonical state.

## 4.5 Replay containment

**VRP-ADF-005**

Invalid replayed operations MUST NOT create new protected execution.

## 4.6 Recovery integrity

**VRP-ADF-006**

Recovery MUST NOT silently restore invalid authority, bypass replay protection or rewrite canonical history.

## 4.7 Deterministic decisions

**VRP-ADF-007**

Given equivalent relevant protocol state and equivalent validated input, a conforming implementation MUST produce decisions consistent with the defined protocol contract.

## 4.8 Evidence accountability

**VRP-ADF-008**

Security validation MUST distinguish observed behavior from expected behavior.

A test result MUST NOT be classified as PASS when required postconditions remain unobserved.

---

# 5. Autonomous Defense Model

Autonomous defense means that the runtime can apply predefined security contracts without requiring a human operator to approve every individual network event.

It does not mean that the runtime possesses unlimited authority to modify protocol state.

Autonomous security decisions must remain bounded by the applicable authorization model.

The public decision categories are:

| Decision | Meaning |
|---|---|
| ACCEPT | Input satisfies the applicable admission contract |
| REJECT | Input violates a required security condition |
| DEFER | Evaluation cannot yet complete under an explicitly defined bounded policy |
| QUARANTINE | Input or candidate state is isolated from protected execution pending permitted evaluation |
| RECOVER | An authorized recovery transition is performed |
| TERMINATE | A session or operation is ended according to an explicit security policy |

These categories are conceptual.

They do not require the implementation to expose these exact internal enum values or state names.

## 5.1 Decision safety

**VRP-ADF-009**

A DEFER or QUARANTINE decision MUST NOT grant protected execution authority.

## 5.2 Decision finality

**VRP-ADF-010**

A rejected operation MUST NOT later become accepted solely because its original rejection record was discarded.

A later operation MAY be accepted if it constitutes a separately valid attempt under the applicable protocol contract.

## 5.3 Bounded uncertainty

**VRP-ADF-011**

Any deferred security decision MUST have documented bounds or a defined terminal outcome.

Indefinite retention of untrusted operations MUST NOT be assumed safe.

---

# 6. Trust Boundary Model

The public autonomous-defense contract distinguishes the following logical responsibilities:

    UNTRUSTED NETWORK INPUT
               |
               v
       INPUT ELIGIBILITY
               |
               v
       ADMISSION DECISION
               |
               v
       AUTHORITY VALIDATION
               |
               v
       CANONICAL STATE
               |
               v
       AUTHORIZED EXECUTION

       RECOVERY AND TRANSPORT
       REPLACEMENT MUST RESPECT
       ALL APPLICABLE BOUNDARIES

This diagram represents security obligations, not a proprietary processing pipeline.

An implementation may use a different internal structure while preserving the externally observable contract.

---

# 7. Unknown Traffic Handling

Unknown traffic is any received input whose protocol eligibility or intended semantics cannot be established under the applicable specification.

Examples include:

- Unsupported message types.
- Unrecognized protocol identifiers.
- Unexpected input sequences.
- Invalid state-dependent requests.
- Unrecognized authorization context.

## 7.1 Required behavior

**VRP-ADF-012**

Unknown traffic MUST NOT acquire execution authority by default.

**VRP-ADF-013**

Unknown traffic MUST NOT cause unauthorized canonical-state mutation.

**VRP-ADF-014**

An unknown endpoint MUST NOT become an authorized persistent transport path solely because it submitted traffic.

**VRP-ADF-015**

Unknown traffic handling SHOULD avoid revealing sensitive internal implementation details through diagnostic responses.

## 7.2 Expected security outcome

    UNKNOWN_INPUT = UNTRUSTED

    UNAUTHORIZED_EXECUTION = FALSE

    UNAUTHORIZED_STATE_COMMIT = FALSE

    UNAUTHORIZED_PATH_ADOPTION = FALSE

## 7.3 Verification

Tests should establish:

1. The input was genuinely unsupported or invalid.
2. The runtime received the input.
3. The applicable rejection occurred.
4. Protected state remained valid.
5. Legitimate traffic remained possible where required.

---

# 8. Malformed Packet Defense

Malformed input may include:

- Truncated packets.
- Invalid lengths.
- Inconsistent fields.
- Unsupported encodings.
- Invalid metadata.
- Unexpected structural combinations.

## 8.1 Required behavior

**VRP-ADF-016**

Malformed packets MUST NOT bypass admission requirements.

**VRP-ADF-017**

Malformed packets MUST NOT trigger unauthorized protected execution.

**VRP-ADF-018**

Malformed input MUST NOT cause an unauthorized persistent authority or session-identity change.

**VRP-ADF-019**

A conforming implementation MUST define its behavior when malformed traffic is received under documented resource limits.

## 8.2 Runtime safety

A malformed-input test SHOULD observe:

- Process survival.
- Memory behavior.
- CPU behavior.
- Admission outcome.
- Canonical-state integrity.
- Legitimate positive-control traffic.

Process survival alone does not establish successful security handling.

## 8.3 Verification limitation

Passing a finite malformed-input corpus does not prove that every possible malformed input has been safely handled.

---

# 9. Replay Storm Defense

Replay traffic may attempt to repeat previously observed operations.

A replay storm may combine high submission volume with duplicate or historical sequence material.

## 9.1 Required behavior

**VRP-ADF-020**

Invalid replayed operations MUST NOT produce new protected canonical commits.

**VRP-ADF-021**

Replay rejection MUST remain consistent with the applicable authority and session identity.

**VRP-ADF-022**

Replay handling MUST preserve the relevant security contract during supported recovery.

## 9.2 Required observations

    REPLAY_ATTEMPTS

    REPLAY_ACCEPTED

    REPLAY_REJECTED

    UNAUTHORIZED_COMMITS

    CANONICAL_STATE_BEFORE

    CANONICAL_STATE_AFTER

## 9.3 Acceptance criteria

For operations already invalidated by the replay contract:

    INVALID_REPLAY_ACCEPTED = 0

    UNAUTHORIZED_DUPLICATE_COMMITS = 0

## 9.4 Public reference model

The VRP public validation kit contains a replay-storm reference scenario involving 9,999 duplicate attempts.

Repository:

https://github.com/Endless33/vrp-validation-kit

Reference-model success does not automatically establish production packet-path enforcement.

---

# 10. Concurrent Duplicate Admission

Concurrency introduces security risks that may not appear during sequential testing.

Multiple execution contexts may attempt to admit the same protected operation.

## 10.1 Required behavior

**VRP-ADF-023**

Concurrent admission MUST preserve the protocol's single-admission contract where that contract applies.

**VRP-ADF-024**

A duplicate operation MUST NOT become valid merely because two or more admission attempts overlap in time.

**VRP-ADF-025**

Concurrent rejection MUST NOT result in unauthorized partial canonical commits.

## 10.2 Verification

Required tests SHOULD include:

- Simultaneous duplicate submission.
- Repeated concurrent execution.
- Legitimate sequence progression.
- Canonical-state comparison.
- Race detection.
- Admission-count verification.

A race-detector PASS is not a proof of linearizability.

The externally observable admission contract must also be verified.

---

# 11. Endpoint Change and Path Adoption

Endpoint changes may occur legitimately because of:

- NAT rebinding.
- Mobile network transitions.
- Interface changes.
- Relay replacement.
- Transport restoration.

An attacker may attempt to exploit the same conditions.

## 11.1 Required behavior

**VRP-ADF-026**

A changed endpoint MUST NOT automatically receive protected execution authority.

**VRP-ADF-027**

Rejected traffic MUST NOT authorize persistent endpoint adoption.

**VRP-ADF-028**

Legitimate endpoint migration MUST satisfy the applicable continuity and authority contracts.

**VRP-ADF-029**

A conforming implementation MUST distinguish transport reachability from authorized session participation.

## 11.2 Required observations

    AUTHORIZED_PATH_BEFORE

    CANDIDATE_PATH

    ADMISSION_DECISION

    AUTHORIZED_PATH_AFTER

    LOGICAL_SESSION_IDENTITY

## 11.3 Acceptance criteria

    REJECTED_PATH_ADOPTED = FALSE

    UNAUTHORIZED_AUTHORITY_TRANSFER = FALSE

    CANONICAL_SESSION_IDENTITY = PRESERVED

The final condition applies when the scenario is intended to preserve the existing logical session.

---

# 12. Stale Authority Defense

Stale authority refers to authority that is no longer valid under the applicable protocol state.

## 12.1 Required behavior

**VRP-ADF-030**

Stale authority MUST NOT commit protected operations.

**VRP-ADF-031**

Transport replacement MUST NOT reactivate stale authority.

**VRP-ADF-032**

Recovery MUST NOT silently restore invalid authority.

## 12.2 Acceptance criteria

    STALE_AUTHORITY_ACCEPTED = FALSE

    UNAUTHORIZED_COMMIT = FALSE

    CANONICAL_AUTHORITY_INTEGRITY = PRESERVED

## 12.3 Verification

Tests SHOULD include:

- Stale authority submission.
- Stale authority during migration.
- Stale authority after blackout.
- Legitimate authority positive control.

---

# 13. Epoch Rollback Defense

An epoch rollback attack attempts to restore an invalid historical authority state.

## 13.1 Required behavior

**VRP-ADF-033**

Invalid historical epochs MUST NOT regain protected execution authority.

**VRP-ADF-034**

Recovery MUST NOT implicitly authorize an invalid rollback.

**VRP-ADF-035**

Rejected rollback attempts MUST NOT rewrite canonical authority history.

## 13.2 Acceptance criteria

    INVALID_ROLLBACK_ACCEPTED = FALSE

    UNAUTHORIZED_AUTHORITY_RESTORATION = FALSE

    CANONICAL_HISTORY_PRESERVED = TRUE

The exact validity rules for epochs are defined by the applicable protocol contract and are not disclosed here as implementation algorithms.

---

# 14. Conflicting Authority

Distributed recovery may expose conflicting authority claims.

A continuity-first runtime must not resolve these conflicts through arbitrary transport arrival order unless such behavior is explicitly authorized by the applicable protocol contract.

## 14.1 Required behavior

**VRP-ADF-036**

Conflicting authority MUST be handled according to a defined and testable resolution contract.

**VRP-ADF-037**

Invalid conflicting authority MUST NOT cause unauthorized canonical-state mutation.

**VRP-ADF-038**

The same declared initial state and relevant input conditions MUST produce outcomes consistent with the specified deterministic contract.

## 14.2 Required observations

- Initial authority state.
- Competing authority inputs.
- Admission outcomes.
- Final canonical authority.
- Repeated-test consistency.

## 14.3 Limitation

This document does not disclose the internal authority-resolution algorithm.

The public requirement concerns observable correctness.

---

# 15. Blackout and Recovery

A blackout is a period during which the relevant transport path is unavailable or unable to deliver required traffic.

Recovery must preserve security independently of whether connectivity returns quickly.

## 15.1 Required behavior

**VRP-ADF-039**

Transport blackout MUST NOT automatically invalidate the logical-session identity when the supported continuity contract requires preservation.

**VRP-ADF-040**

Recovery MUST NOT bypass admission requirements.

**VRP-ADF-041**

Recovery MUST NOT reset replay protections in a manner that permits invalid historical execution.

**VRP-ADF-042**

Recovery MUST NOT restore invalid authority.

**VRP-ADF-043**

A claimed successful continuity result MUST be supported by an independent logical-session identity observation.

## 15.2 Proposed test profiles

| Profile | Blackout |
|---|---|
| VRP-BL-010 | 10 seconds |
| VRP-BL-030 | 30 seconds |
| VRP-BL-060 | 60 seconds |

These profiles are proposed qualification scenarios.

They are not claims of completed runtime validation.

## 15.3 Required measurements

    SESSION_IDENTITY_BEFORE

    SESSION_IDENTITY_AFTER

    TRANSPORT_STATE_BEFORE

    TRANSPORT_STATE_AFTER

    AUTHORITY_STATE

    CANONICAL_STATE

    RECOVERY_DURATION

    APPLICATION_PROGRESS

## 15.4 Verdict

A restored socket or successful packet exchange alone MUST NOT be accepted as proof of logical-session continuity.

---

# 16. Packet Loss, Delay and Reordering

Unstable networks may introduce:

- Packet loss.
- Delayed delivery.
- Reordering.
- Duplicate arrival.
- Temporary path unavailability.

## 16.1 Required behavior

**VRP-ADF-044**

Packet loss MUST NOT create unauthorized authority.

**VRP-ADF-045**

Reordering MUST NOT cause invalid historical operations to bypass the applicable replay or authority contract.

**VRP-ADF-046**

Delayed traffic following recovery MUST remain subject to the applicable admission requirements.

## 16.2 Proposed validation profiles

    LOSS = 30%

    LOSS = 50%

    LOSS = 80%

    LOSS = 90%

    RTT = 300-900 MS

    JITTER = ENABLED

    REORDERING = ENABLED

These are test configurations, not performance guarantees.

## 16.3 Required evidence

- Delivered application operations.
- Rejected invalid operations.
- Session identity.
- Authority state.
- Recovery time.
- Resource consumption.
- Explicit operating conditions.

---

# 17. Compound Adversarial Conditions

Single-fault success does not establish multi-fault correctness.

A compound scenario may combine:

- Endpoint churn.
- High packet loss.
- Blackout.
- Replay.
- Delayed traffic.
- Authority contention.

## 17.1 Proposed compound profile

    ENDPOINT_CHURN_INTERVAL = 2 SECONDS

    PACKET_LOSS = 80%

    BLACKOUT = ENABLED

    REPLAY_PRESSURE = ENABLED

    AUTHORITY_CONFLICT = CONTROLLED

## 17.2 Required behavior

**VRP-ADF-047**

Compound disruption MUST NOT permit unauthorized protected execution.

**VRP-ADF-048**

Invalid authority MUST remain invalid throughout recovery.

**VRP-ADF-049**

Replay containment MUST remain consistent with the applicable contract.

**VRP-ADF-050**

A continuity PASS MUST require independent verification of logical-session identity.

## 17.3 Qualification

Each compound profile MUST have its own evidence record.

Success in one profile MUST NOT be generalized to all possible combinations of failures.

---

# 18. Resource Pressure and Defensive Bounds

Autonomous security decisions consume computational resources.

An attacker may attempt to exploit this by generating sustained invalid traffic.

## 18.1 Required behavior

**VRP-ADF-051**

A conforming implementation MUST document applicable resource limits for its evaluated configuration.

**VRP-ADF-052**

Resource pressure MUST NOT cause unauthorized admission.

**VRP-ADF-053**

Resource exhaustion handling MUST preserve the applicable fail-closed security contract.

**VRP-ADF-054**

Deferred or quarantined input MUST NOT accumulate without documented bounds.

## 18.2 Required measurements

- Input rate.
- Execution duration.
- Memory consumption.
- CPU consumption.
- Queue utilization where available.
- Rejection rate.
- Legitimate traffic progress.
- Recovery behavior.

## 18.3 Important limitation

Rejecting invalid traffic does not establish universal denial-of-service resistance.

An implementation may preserve integrity while availability degrades.

These outcomes MUST be reported separately.

---

# 19. Decision Determinism

Autonomous security decisions must be explainable through the applicable protocol contract.

Determinism does not require every deployment to produce identical timing or scheduling behavior.

It requires consistency of the relevant security decision under equivalent declared conditions.

## 19.1 Required behavior

**VRP-ADF-055**

Equivalent relevant state and validated input MUST produce decisions consistent with the specified contract.

**VRP-ADF-056**

A security decision MUST NOT depend on unauthorized transport identity substitution.

**VRP-ADF-057**

A rejected operation MUST NOT acquire authority through an unrelated recovery event.

## 19.2 Verification

Tests SHOULD record:

- Initial state identity.
- Input classification.
- Expected decision.
- Observed decision.
- Canonical-state postcondition.
- Repeatability result.

## 19.3 Limitation

This contract does not require publication of private internal state or proprietary decision algorithms.

---

# 20. Fail-Closed Versus Availability

Fail-closed behavior may reduce availability under uncertain or hostile conditions.

This is an intentional security tradeoff where authority cannot be established.

The runtime MUST NOT silently convert uncertainty into authorization.

However, fail-closed behavior does not require terminating every legitimate session whenever an unrelated invalid packet is received.

## 20.1 Required distinction

**Rejecting an invalid operation**

Does not necessarily imply rejecting all valid operations.

**Rejecting an unauthorized endpoint**

Does not necessarily imply terminating an existing authorized session.

**Rejecting stale authority**

Does not necessarily imply discarding valid canonical history.

**Failing to recover**

Does not authorize relaxing security requirements.

## 20.2 Required behavior

**VRP-ADF-058**

An implementation MUST NOT grant protected authority solely to improve availability.

**VRP-ADF-059**

An implementation SHOULD preserve legitimate operation when this is possible without violating the security contract.

**VRP-ADF-060**

Availability degradation and security-integrity violations MUST be reported as distinct outcomes.

---

# 21. Decision Observability

A security decision that cannot be independently evaluated provides limited assurance.

The public contract therefore defines minimum observable evidence.

## 21.1 Required decision record

A test record SHOULD contain:

    TEST_ID

    SOURCE_REVISION

    INPUT_CLASS

    INITIAL_STATE_REFERENCE

    EXPECTED_DECISION

    OBSERVED_DECISION

    AUTHORITY_POSTCONDITION

    CANONICAL_STATE_POSTCONDITION

    SESSION_IDENTITY_POSTCONDITION

    EXECUTION_TIMESTAMP

    VERDICT

## 21.2 Confidentiality

Public evidence MUST NOT expose:

- Secret keys.
- Authentication credentials.
- Proprietary decision algorithms.
- Protected runtime implementation.
- Confidential deployment details.

## 21.3 Evidence integrity

Finalized evidence SHOULD be protected by integrity checks.

A checksum alone does not establish the identity of the evidence producer.

---

# 22. Autonomous Defense Decision Matrix

The following table summarizes expected behavior.

| Event | Required security response | Protected property |
|---|---|---|
| Unknown input | Deny unauthorized execution | Admission integrity |
| Malformed packet | Reject invalid operation | Canonical integrity |
| Invalid replay | Reject duplicate execution | Replay containment |
| Concurrent duplicate | Prevent duplicate commit | Admission atomicity |
| Rejected endpoint | Prevent unauthorized adoption | Path integrity |
| Stale authority | Reject protected mutation | Authority integrity |
| Invalid epoch rollback | Reject historical authority restoration | Epoch integrity |
| Conflicting authority | Apply defined resolution contract | Canonical authority |
| Transport blackout | Preserve security during recovery | Session and authority integrity |
| Delayed historical traffic | Apply current validity contract | Replay and authority integrity |
| Resource pressure | Preserve security within documented limits | Fail-closed behavior |
| Compound disruption | Enforce all applicable invariants | Cross-boundary integrity |

This matrix defines requirements.

It is not a table of completed PASS results.

---

# 23. Conformance Test Requirements

An implementation claiming conformance to this contract MUST provide attributable evidence for the applicable mandatory requirements.

## 23.1 Source identity

The evaluated implementation MUST be identified.

## 23.2 Negative testing

Prohibited operations MUST be tested.

## 23.3 Positive controls

Legitimate behavior MUST be tested where required.

## 23.4 Canonical-state observation

Protected-state postconditions MUST be observed.

## 23.5 Recovery verification

Continuity claims MUST use an independent session-identity oracle.

## 23.6 Resource qualification

Claims about bounded behavior MUST include measured resource evidence.

## 23.7 Evidence integrity

Formal results MUST be attributable to the evaluated revision.

---

# 24. Conformance Verdicts

| Verdict | Definition |
|---|---|
| PASS | All applicable mandatory conditions satisfied |
| FAIL | At least one mandatory condition violated |
| INCONCLUSIVE | Evidence insufficient |
| NOT RUN | Test not executed |
| BLOCKED | Test prevented by documented prerequisite |
| MODEL PASS | Reference model passed within its scope |
| RUNTIME PASS | Identified runtime passed within its scope |
| INTEGRATED PASS | Integrated system passed within its scope |

A MODEL PASS MUST NOT be represented as an INTEGRATED PASS.

A RUNTIME PASS MUST NOT be represented as third-party certification unless an independent assessment actually occurred.

---

# 25. Independent Session Identity Oracle

The foundational VRP claim requires independent verification.

**SESSION != TRANSPORT**

The following observations are insufficient by themselves:

- Process survival.
- Socket survival.
- UDP reconnection.
- Tunnel restoration.
- Endpoint replacement.
- Successful packet delivery.

## 25.1 Required observation tuple

    LOGICAL_SESSION_IDENTITY

    TRANSPORT_IDENTITY

    AUTHORITY_STATE

    CANONICAL_STATE

    APPLICATION_PROGRESS

    RECOVERY_TIMESTAMP

## 25.2 Acceptance criteria

The evaluator MUST establish whether the same logical session persists across the supported transport transition.

The evaluator MUST distinguish preserved identity from creation of a replacement session with equivalent-looking behavior.

The applicable authority and canonical-state invariants MUST remain satisfied.

---

# 26. Security Regression and RED-to-GREEN

A security correction must be supported by a reproducible verification chain.

    INSPECT
       |
       v
    DEFINE INVARIANT
       |
       v
    RED — REPRODUCE FAILURE
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
    RECORD VERDICT

## 26.1 Required evidence

    FINDING_ID

    INVARIANT_ID

    RED_SOURCE_REVISION

    RED_EVIDENCE_REFERENCE

    CORRECTION_REVISION

    GREEN_EVIDENCE_REFERENCE

    REGRESSION_RESULT

    RESIDUAL_RISK

## 26.2 Closure requirement

A security finding MUST NOT be classified as verified closed solely because a correction commit exists.

The original failure, correction and relevant regression results must be attributable.

---

# 27. Autonomous Defense Acceptance Gates

A formal adversarial PASS requires the applicable gates below.

## GATE 1 — Identified Source

The implementation revision is known.

## GATE 2 — Verified Preconditions

The required initial state is established.

## GATE 3 — Applied Adversarial Input

The intended stimulus is demonstrably applied.

## GATE 4 — Admission Integrity

Invalid input does not acquire authority.

## GATE 5 — Authority Integrity

Unauthorized authority does not commit protected changes.

## GATE 6 — Canonical Integrity

Rejected operations do not corrupt protected state.

## GATE 7 — Continuity Integrity

Where continuity is claimed, logical identity is independently verified.

## GATE 8 — Positive Control

Legitimate operation remains valid where required.

## GATE 9 — Evidence Integrity

The result is attributable and internally consistent.

## GATE 10 — Scope Declaration

Limitations and untested conditions are explicit.

A missing required gate MUST result in an appropriately limited verdict.

---

# 28. Deployment Considerations

Autonomous defensive behavior may be relevant to:

- Mobile infrastructure.
- Enterprise networking.
- Distributed services.
- Intermittent wireless networks.
- Satellite-like communications.
- Relay-based transport environments.
- Long-lived application sessions.

Each deployment requires its own threat assumptions, operating limits and acceptance criteria.

This document does not claim certification for telecom core networks, satellite infrastructure, government systems or safety-critical environments.

---

# 29. Residual Risks

## 29.1 Unknown vulnerabilities

No finite test suite establishes universal security.

## 29.2 Host compromise

A compromised trusted host may invalidate assumptions outside the network-facing protocol contract.

## 29.3 Credential compromise

Valid stolen credentials may require separate revocation and incident-response controls.

## 29.4 Availability exhaustion

Fail-closed admission does not guarantee availability under unlimited traffic pressure.

## 29.5 Integration defects

Application and operating-system integration may introduce failures outside the reference-model scope.

## 29.6 Incomplete continuity evidence

Transport recovery without independent session-identity observation is insufficient to prove continuity.

## 29.7 Deployment variation

Observed behavior may depend on configuration, runtime version and network conditions.

## 29.8 Incomplete adversarial coverage

Successful isolated tests do not establish compound-failure correctness.

---

# 30. Public Assurance Position

VRP's public autonomous-defense contract is based on:

- Explicit security invariants.
- Defined admission requirements.
- Authority separation.
- Canonical-state integrity.
- Replay containment.
- Recovery preservation.
- Deterministic decision requirements.
- Reproducible adversarial testing.
- Evidence-based qualification.

The public validation kit provides independently executable reference-model scenarios for selected properties.

Repository:

https://github.com/Endless33/vrp-validation-kit

The public validation kit does not automatically establish integrated runtime conformance.

Runtime and deployment qualification require separate evidence.

---

# 31. Protected Core Disclosure Boundary

This document intentionally exposes:

- Security requirements.
- Observable decision categories.
- Threat classes.
- Expected rejection behavior.
- Conformance criteria.
- Evidence requirements.
- Residual limitations.

This document intentionally excludes:

- Proprietary decision algorithms.
- Private authority-resolution logic.
- Internal state-machine implementation.
- Protected continuity mechanisms.
- Secret cryptographic material.
- Confidential packet-processing internals.
- Private runtime source code.
- Non-public deployment configuration.

**PROTECTED CORE DISCLOSURE: NONE**

The public contract defines what must be verified, not how proprietary internal mechanisms are implemented.

---

# 32. Related Documentation

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

## Public Validation Kit

https://github.com/Endless33/vrp-validation-kit

## VRP Project

https://github.com/Endless33/Veil-Routing-Protocol

All referenced artifacts should be verified against their actual published repository revisions.

---

# 33. Final Engineering Statement

A continuity-first protocol must be evaluated under conditions where the network is no longer trustworthy or predictable.

Packets may be malformed.

Traffic may be duplicated.

Endpoints may change.

Authority may become stale.

Recovery may be interrupted.

Multiple failures may occur simultaneously.

Under these conditions, autonomous defense must preserve the security contract.

The intended VRP requirements are:

**SESSION != TRANSPORT**

**REACHABILITY != AUTHORITY**

**UNKNOWN INPUT != TRUSTED INPUT**

**REPLAY != NEW EXECUTION**

**REJECTION != PARTIAL COMMIT**

**RECOVERY != AUTHORITY RESET**

**AUTONOMY != UNLIMITED AUTHORITY**

**TRANSPORT RESTORATION != VERIFIED CONTINUITY**

**SECURITY CLAIM != VERIFIED SECURITY EVIDENCE**

The architectural objective is:

**AUTONOMOUS CONTINUITY WITH BOUNDED AUTHORITY AND FAIL-CLOSED SECURITY.**

The engineering requirement is:

**WHEN THE NETWORK BECOMES HOSTILE, THE SECURITY CONTRACT MUST REMAIN IN FORCE.**

---

**END OF DOCUMENT**

**Veil Routing Protocol — VRP**

**Author:** Vitalijus Riabovas

© 2026 Vitalijus Riabovas. All rights reserved.

This public document specifies proposed security behavior, verification requirements and assurance boundaries. It does not disclose proprietary implementation mechanisms or grant access to Protected Core materials. Applicable repository license terms govern reuse.
