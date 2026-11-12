
# VRP THREAT MODEL & TRUST BOUNDARIES

## Veil Routing Protocol (VRP)
### Security Architecture, Adversarial Capabilities and Trust Boundary Specification

**Document ID:** VRP-TMTB-004  
**Version:** 1.0  
**Date:** 2026-11-12  
**Status:** Public Security Architecture Review  
**Classification:** PUBLIC — NON-PROPRIETARY  
**Author:** Vitalijus Riabovas  
**Project:** Veil Routing Protocol (VRP)

---

# 1. Executive Summary

The Veil Routing Protocol (VRP) is an experimental continuity-first networking protocol based on a fundamental architectural separation:

**SESSION != TRANSPORT**

The logical session is not defined solely by the lifetime, address or availability of an individual network transport.

This separation introduces an important security requirement.

A session may remain logically continuous while its underlying transport changes, but the transition must not create an opportunity for unauthorized execution, authority escalation, replay acceptance or canonical-state corruption.

VRP therefore treats session continuity, transport reachability and execution authority as distinct security concerns.

The central security objective is:

**PRESERVE CONTINUITY WITHOUT TRANSFERRING UNAUTHORIZED AUTHORITY.**

This document establishes the public threat model and trust-boundary requirements for VRP.

It defines:

- Protected assets.
- Security principals.
- Adversarial capabilities.
- Trust assumptions.
- Logical trust boundaries.
- STRIDE threat categories.
- Authority abuse scenarios.
- Replay and duplicate-execution threats.
- Endpoint migration threats.
- Recovery and rollback threats.
- Concurrent execution threats.
- Availability and resource-exhaustion threats.
- Security acceptance criteria.
- Residual risks and assurance limitations.

The document describes observable security contracts rather than proprietary implementation mechanisms.

It does not constitute a formal security proof, third-party certification or production deployment authorization.

---

# 2. Architectural Security Principles

VRP's public security architecture is organized around six principles.

## 2.1 Session identity is not transport identity

A logical session must not be defined exclusively by:

- IP address.
- UDP endpoint.
- TCP connection.
- Network interface.
- Relay identity.
- Process identifier.
- Socket lifetime.

A transport change must not automatically imply creation of a new logical session.

Equally, transport restoration must not automatically be accepted as proof that the original logical session survived.

## 2.2 Reachability does not grant authority

A reachable endpoint is not necessarily an authorized endpoint.

The ability to deliver packets does not establish permission to modify protected protocol state.

**TRANSPORT REACHABILITY != EXECUTION AUTHORITY**

## 2.3 Recovery does not reset security

Network recovery must not silently reset:

- Replay protection.
- Authority validity.
- Canonical history.
- Protected session identity.
- Security admission requirements.

**RECOVERY != AUTHORITY RESET**

## 2.4 Rejected operations must not commit

A rejected operation must not produce unauthorized protected-state mutation.

The rejection contract applies to the operation's relevant security effects, not merely its returned error status.

**REJECTION != PARTIAL COMMIT**

## 2.5 Authority must remain attributable

Protected state transitions must be attributable to an accepted authority context.

Conflicting, stale or invalid authority must not obtain execution rights through transport changes or recovery.

## 2.6 Security claims require evidence

Security properties must be evaluated through reproducible tests, observable postconditions and attributable evidence.

A passing test establishes only the properties evaluated within its declared scope.

---

# 3. Scope and System Boundaries

This threat model covers the public VRP security contract across the following logical concerns:

1. Network-facing input.
2. Protocol admission.
3. Logical-session identity.
4. Authority validation.
5. Canonical-state integrity.
6. Transport replacement.
7. Recovery.
8. Application-visible execution.
9. Security evidence.

These concerns are conceptual boundaries.

They do not prescribe internal module structure, implementation topology or proprietary algorithms.

## 3.1 In scope

The threat model considers:

- Malicious packet injection.
- Malformed input.
- Packet duplication.
- Replay.
- Network-path manipulation.
- Endpoint spoofing attempts.
- Stale authority.
- Authority rollback.
- Competing authority.
- Unauthorized canonical-state mutation.
- Concurrent admission.
- Recovery manipulation.
- Resource exhaustion.
- Evidence tampering.

## 3.2 Out of scope

The baseline document does not establish protection against:

- Complete compromise of a trusted host.
- Theft of valid endpoint credentials.
- Compromise of an authorized administrator.
- Physical attacks on infrastructure.
- Side-channel attacks.
- Cryptographic implementation vulnerabilities.
- Supply-chain compromise.
- Malicious modifications to trusted binaries.
- Universal denial-of-service resistance.
- All possible traffic-analysis attacks.

These risks may require separate controls or additional assessment.

Exclusion from this threat model does not imply that a threat is impossible or unimportant.

---

# 4. Protected Assets

The security assessment identifies the following protected assets.

| Asset ID | Protected asset | Security objective |
|---|---|---|
| VRP-AST-001 | Logical Session Identity | Prevent unauthorized identity replacement |
| VRP-AST-002 | Execution Authority | Prevent unauthorized protected operations |
| VRP-AST-003 | Canonical State | Prevent unauthorized state mutation |
| VRP-AST-004 | Canonical History | Prevent unauthorized historical modification |
| VRP-AST-005 | Replay State | Prevent invalid repeated execution |
| VRP-AST-006 | Authority Epoch State | Prevent invalid historical authority restoration |
| VRP-AST-007 | Transport Authorization State | Prevent unauthorized endpoint adoption |
| VRP-AST-008 | Recovery State | Preserve security across disruption |
| VRP-AST-009 | Application Execution Integrity | Prevent unauthorized application-visible effects |
| VRP-AST-010 | Security Evidence | Preserve attributable verification results |
| VRP-AST-011 | Runtime Resources | Maintain documented operational bounds |

Confidentiality of payload data and metadata is deployment-dependent and requires separately specified cryptographic and operational controls.

---

# 5. Security Principals

The threat model distinguishes conceptual principals rather than assuming that network addresses uniquely identify participants.

## 5.1 Authorized session participant

An entity authorized to participate in a logical session under the applicable protocol security contract.

## 5.2 Authorized execution principal

An entity permitted to request a protected state transition or application-visible operation.

## 5.3 Transport endpoint

A network endpoint used to carry protocol traffic.

An endpoint is not inherently an execution principal.

## 5.4 Recovery participant

An entity involved in restoring connectivity or session operation after disruption.

Recovery participation does not independently establish execution authority.

## 5.5 External attacker

An unauthorized entity attempting to influence protocol behavior through accessible interfaces.

## 5.6 Privileged attacker

An attacker possessing some legitimate access, compromised credentials or partial authority.

The exact capabilities of such an attacker depend on the evaluated deployment.

## 5.7 Independent verifier

An external evaluator examining public protocol behavior, test evidence and declared security postconditions.

A verifier's ability to observe a security property must not be confused with authority to modify the protected system.

---

# 6. Attacker Capability Model

Threat analysis must distinguish between assumed attacker capabilities and capabilities demonstrated in an actual test.

## 6.1 Network injection

The attacker may submit arbitrary traffic to a reachable network-facing interface.

Potential objectives include:

- Malformed-input exploitation.
- Unauthorized admission.
- State confusion.
- Resource exhaustion.

## 6.2 Replay

The attacker may retransmit previously observed traffic.

Potential objectives include:

- Duplicate execution.
- Repeated authorization.
- Replay-state manipulation.
- Recovery-state confusion.

## 6.3 Reordering and delay

The attacker may influence packet arrival order or timing in a controlled test environment.

Potential objectives include:

- Stale-state acceptance.
- Incorrect transition ordering.
- Invalid authority restoration.

## 6.4 Endpoint manipulation

The attacker may submit traffic that appears to originate from a changed endpoint, subject to the capabilities of the evaluated network.

Potential objectives include:

- Unauthorized path adoption.
- Session hijacking.
- Authority confusion.

## 6.5 Concurrent execution

The attacker may trigger multiple admission attempts simultaneously.

Potential objectives include:

- Duplicate acceptance.
- State races.
- Non-atomic authorization behavior.

## 6.6 Availability pressure

The attacker may generate high volumes of invalid or duplicate traffic.

Potential objectives include:

- Resource exhaustion.
- Admission degradation.
- Recovery starvation.

## 6.7 Partial compromise

A separate assessment may consider an attacker controlling a limited authorized component.

Such scenarios require explicit assumptions regarding compromised privileges, credential access and trust relationships.

They must not be silently included in conclusions from unauthenticated network testing.

---

# 7. Trust Boundary Architecture

The public VRP security model distinguishes five primary logical boundaries.

    +-----------------------------------+
    |       UNTRUSTED NETWORK           |
    |                                   |
    |  Injection / Replay / Spoofing    |
    +----------------+------------------+
                     |
                     v
    +-----------------------------------+
    | TB-01: PROTOCOL ADMISSION         |
    |                                   |
    | Input validation and eligibility  |
    +----------------+------------------+
                     |
                     v
    +-----------------------------------+
    | TB-02: AUTHORITY VALIDATION       |
    |                                   |
    | Permission for protected actions  |
    +----------------+------------------+
                     |
                     v
    +-----------------------------------+
    | TB-03: CANONICAL STATE            |
    |                                   |
    | Protected state transition        |
    +----------------+------------------+
                     |
                     v
    +-----------------------------------+
    | TB-04: AUTHORIZED EXECUTION       |
    |                                   |
    | Application-visible effects       |
    +-----------------------------------+

    TB-05: RECOVERY AND TRANSPORT CHANGE
           CROSS-CUTTING SECURITY BOUNDARY

This diagram represents logical security responsibilities.

It is not an implementation data-flow diagram.

The actual runtime may organize these responsibilities differently while preserving the same externally testable security contract.

---

# 8. Trust Boundary TB-01: Network to Admission

## 8.1 Boundary definition

Traffic originating from an untrusted network must not be treated as authorized solely because it reaches a protocol endpoint.

## 8.2 Threats

- Malformed packet submission.
- Unsupported message types.
- Invalid metadata.
- Packet flooding.
- Unexpected state-transition requests.
- Protocol parsing abuse.

## 8.3 Security requirements

**VRP-TB01-REQ-001**

Invalid inputs must not obtain protected execution authority.

**VRP-TB01-REQ-002**

Malformed inputs must not cause unauthorized canonical-state mutation.

**VRP-TB01-REQ-003**

Admission rejection must not silently authorize endpoint migration.

**VRP-TB01-REQ-004**

Admission behavior must remain consistent with documented resource limits.

## 8.4 Required verification

- Negative input tests.
- Rejection-state inspection.
- Positive controls.
- Resource measurements where applicable.

---

# 9. Trust Boundary TB-02: Admission to Authority

## 9.1 Boundary definition

Passing an input-validation boundary does not automatically establish permission to perform a protected operation.

## 9.2 Threats

- Stale authority.
- Invalid authority context.
- Conflicting authority.
- Replay of previously valid operations.
- Unauthorized operation escalation.

## 9.3 Security requirements

**VRP-TB02-REQ-001**

Protected operations must satisfy the applicable authority contract.

**VRP-TB02-REQ-002**

Stale authority must not commit protected changes.

**VRP-TB02-REQ-003**

Invalid historical authority must not regain execution rights through recovery.

**VRP-TB02-REQ-004**

A transport change must not independently grant execution authority.

## 9.4 Required verification

- Stale-authority rejection.
- Invalid-epoch rejection.
- Conflicting-authority tests.
- Protected-state postcondition checks.

---

# 10. Trust Boundary TB-03: Authority to Canonical State

## 10.1 Boundary definition

Authority validation must not be bypassed by direct or indirect state mutation.

## 10.2 Threats

- Unauthorized commit.
- Duplicate commit.
- Partial commit after rejection.
- Canonical-history rewriting.
- Concurrent state mutation.

## 10.3 Security requirements

**VRP-TB03-REQ-001**

Rejected operations must not commit unauthorized protected-state changes.

**VRP-TB03-REQ-002**

Duplicate operations must not create multiple canonical commits where the contract permits only one.

**VRP-TB03-REQ-003**

Canonical history must resist unauthorized modification.

**VRP-TB03-REQ-004**

Concurrent execution must preserve the declared state-transition contract.

## 10.4 Required verification

- Before-and-after canonical-state comparison.
- Duplicate admission tests.
- Concurrency tests.
- Regression verification.
- Explicit accepted and rejected operation counts.

---

# 11. Trust Boundary TB-04: Canonical State to Execution

## 11.1 Boundary definition

Application-visible execution must be consistent with accepted canonical state.

## 11.2 Threats

- Execution without accepted authority.
- Duplicate application-visible effects.
- Execution following rejected admission.
- Recovery-induced duplicate processing.

## 11.3 Security requirements

**VRP-TB04-REQ-001**

Unauthorized input must not produce protected application-visible execution.

**VRP-TB04-REQ-002**

Duplicate delivery must not violate the application's declared execution contract.

**VRP-TB04-REQ-003**

Recovery must not silently create unauthorized execution.

## 11.4 Important limitation

Exactly-once application execution is not established merely by packet replay rejection.

Application-level idempotency, commit semantics and failure handling require independent evaluation.

VRP's transport-level and protocol-level security claims must not be represented as universal application exactly-once guarantees.

---

# 12. Trust Boundary TB-05: Recovery and Transport Replacement

## 12.1 Boundary definition

Recovery and transport migration must preserve the security contract established before disruption.

## 12.2 Threats

- Unauthorized endpoint adoption.
- Session identity substitution.
- Replay after blackout.
- Stale authority restoration.
- Epoch rollback.
- Competing recovery authority.
- Canonical-history corruption.

## 12.3 Security requirements

**VRP-TB05-REQ-001**

Supported transport replacement must not automatically replace logical-session identity.

**VRP-TB05-REQ-002**

Rejected traffic must not authorize a new endpoint.

**VRP-TB05-REQ-003**

Recovery must not invalidate replay-containment guarantees.

**VRP-TB05-REQ-004**

Recovery must not authorize obsolete authority.

**VRP-TB05-REQ-005**

Continuity claims require independent logical-session identity evidence.

## 12.4 Required verification

- Independent session-identity oracle.
- Endpoint identity observations.
- Authority-state observations.
- Replay rejection during recovery.
- Canonical-state comparisons.
- Application progress measurements.

---

# 13. STRIDE Threat Analysis

STRIDE provides a structured classification of security threats.

The categories are:

- Spoofing.
- Tampering.
- Repudiation.
- Information Disclosure.
- Denial of Service.
- Elevation of Privilege.

## 13.1 STRIDE mapping

| STRIDE | VRP threat | Primary boundary |
|---|---|---|
| Spoofing | Unauthorized endpoint impersonation | TB-01, TB-05 |
| Spoofing | Session identity substitution | TB-05 |
| Tampering | Unauthorized canonical-state mutation | TB-03 |
| Tampering | Replay-state manipulation | TB-02, TB-03 |
| Tampering | Invalid authority rollback | TB-02, TB-05 |
| Repudiation | Unattributable security decision | Evidence boundary |
| Information Disclosure | Traffic metadata exposure | Network boundary |
| Denial of Service | Invalid traffic flooding | TB-01 |
| Denial of Service | Recovery starvation | TB-05 |
| Elevation of Privilege | Transport reachability treated as authority | TB-02 |
| Elevation of Privilege | Stale authority accepted | TB-02 |
| Elevation of Privilege | Unauthorized commit | TB-03 |

STRIDE classification identifies threats.

It does not establish that each threat has been fully mitigated.

---

# 14. Threat Register

Each threat receives a stable identifier.

| Threat ID | Threat | Primary asset | Required response |
|---|---|---|---|
| VRP-THR-001 | Endpoint spoofing | AST-007 | Reject unauthorized adoption |
| VRP-THR-002 | Replay injection | AST-005 | Reject invalid replay |
| VRP-THR-003 | Duplicate commit | AST-003 | Prevent duplicate canonical execution |
| VRP-THR-004 | Stale authority | AST-002 | Reject obsolete authority |
| VRP-THR-005 | Epoch rollback | AST-006 | Reject invalid historical authority |
| VRP-THR-006 | Canonical-history tampering | AST-004 | Preserve protected history |
| VRP-THR-007 | Recovery hijacking | AST-008 | Preserve recovery authority integrity |
| VRP-THR-008 | Session substitution | AST-001 | Preserve canonical session identity |
| VRP-THR-009 | Concurrent admission race | AST-005 | Preserve admission atomicity |
| VRP-THR-010 | Malformed input | AST-003 | Fail closed |
| VRP-THR-011 | Resource exhaustion | AST-011 | Enforce documented bounds |
| VRP-THR-012 | Evidence manipulation | AST-010 | Detect inconsistent evidence |
| VRP-THR-013 | Duplicate application effect | AST-009 | Enforce applicable execution contract |
| VRP-THR-014 | Recovery starvation | AST-008 | Evaluate bounded recovery behavior |
| VRP-THR-015 | Unauthorized authority transfer | AST-002 | Preserve authority separation |

The register defines security assessment targets.

It is not a claim that every listed threat has been independently tested or eliminated.

---

# 15. Replay and Duplicate Execution Threats

## 15.1 Threat description

An attacker retransmits previously observed operations or causes duplicate submissions under concurrent execution.

## 15.2 Potential impact

- Duplicate canonical commit.
- Repeated protected execution.
- Replay-state inconsistency.
- Invalid authority reuse.
- Application-visible duplication.

## 15.3 Required security behavior

Invalid duplicate operations must not produce unauthorized canonical progress.

Replay rejection must remain effective under the declared concurrency and recovery conditions.

## 15.4 Acceptance criteria

    INVALID_REPLAY_ACCEPTED = FALSE

    UNAUTHORIZED_DUPLICATE_COMMIT = FALSE

    CANONICAL_STATE_INTEGRITY = PRESERVED

The exact replay eligibility contract must be established by the applicable public specification.

## 15.5 Verification

Relevant testing includes:

- Duplicate packet storms.
- Historical sequence replay.
- Concurrent duplicate admission.
- Replay during recovery.
- Positive-control admission.

---

# 16. Endpoint Hijacking and Migration Threats

## 16.1 Threat description

An attacker attempts to exploit endpoint changes or transport migration to obtain unauthorized control of a logical session.

## 16.2 Potential impact

- Unauthorized endpoint adoption.
- Session hijacking.
- Traffic redirection.
- Authority confusion.
- Loss of canonical identity.

## 16.3 Required security behavior

A rejected packet must not cause the system to adopt an unauthorized endpoint.

An endpoint change must not independently grant protected execution authority.

## 16.4 Acceptance criteria

    REJECTED_ENDPOINT_ADOPTED = FALSE

    UNAUTHORIZED_AUTHORITY_TRANSFER = FALSE

    CANONICAL_SESSION_IDENTITY = PRESERVED

## 16.5 Verification

- Candidate endpoint injection.
- Rejected path adoption tests.
- NAT rebinding scenarios.
- Transport replacement.
- Independent logical-session identity observation.

---

# 17. Authority Abuse and Rollback Threats

## 17.1 Threat description

An attacker attempts to reuse obsolete authority or induce invalid restoration of a historical authority state.

## 17.2 Potential impact

- Unauthorized state mutation.
- Conflicting authority.
- Invalid rollback.
- Canonical-history corruption.

## 17.3 Required security behavior

Obsolete authority must remain invalid under the applicable authority contract.

Recovery must not silently reactivate invalid authority.

## 17.4 Acceptance criteria

    STALE_AUTHORITY_ACCEPTED = FALSE

    INVALID_EPOCH_ROLLBACK = FALSE

    UNAUTHORIZED_COMMIT = FALSE

## 17.5 Verification

- Stale-authority regression tests.
- Epoch rollback tests.
- Conflicting authority tests.
- Recovery authority integrity tests.

---

# 18. Recovery Attack Surface

Recovery is a security-sensitive phase because the system may experience:

- Missing packets.
- Delayed packets.
- Changed endpoints.
- Temporary transport loss.
- Reordered traffic.
- Competing state observations.

The security contract must remain valid throughout the recovery process.

## 18.1 Recovery threats

**VRP-REC-001**

Replay acceptance after blackout.

**VRP-REC-002**

Unauthorized authority restoration.

**VRP-REC-003**

Canonical session identity substitution.

**VRP-REC-004**

Unauthorized endpoint adoption.

**VRP-REC-005**

Duplicate execution during restoration.

## 18.2 Required observations

    SESSION_IDENTITY_BEFORE

    SESSION_IDENTITY_AFTER

    AUTHORITY_BEFORE

    AUTHORITY_AFTER

    CANONICAL_STATE_BEFORE

    CANONICAL_STATE_AFTER

    REPLAY_DECISIONS

    APPLICATION_PROGRESS

## 18.3 Verification limitation

Transport reconnection alone does not establish successful secure recovery.

The independent session-identity oracle and authority-state observations must support the verdict.

---

# 19. Concurrency Threats

Concurrent execution can expose security defects not observable in sequential tests.

## 19.1 Threat categories

- Simultaneous replay admission.
- Duplicate operation races.
- Concurrent authority changes.
- Admission during recovery.
- Inconsistent state observation.

## 19.2 Security requirement

Operations that are defined as single-commit operations must preserve that contract under concurrent execution.

## 19.3 Verification

- Concurrent regression tests.
- Race detection.
- Repeated scheduling.
- Accepted/rejected count verification.
- Canonical-state comparison.

A race-detector PASS is useful evidence but does not independently prove linearizability.

---

# 20. Denial-of-Service and Resource Exhaustion

## 20.1 Threat description

An attacker generates traffic intended to exhaust protocol resources or prevent legitimate progress.

## 20.2 Potential impact

- Memory exhaustion.
- CPU saturation.
- Queue overload.
- Recovery starvation.
- Legitimate traffic degradation.

## 20.3 Security requirements

Runtime resource usage must remain within documented bounds for the evaluated operating conditions.

Invalid traffic must not silently bypass admission or authority validation under pressure.

## 20.4 Required measurements

- Packet rate.
- Duration.
- CPU consumption.
- Memory consumption.
- Queue utilization where available.
- Rejection rate.
- Legitimate delivery.
- Recovery time.

## 20.5 Limitation

Functional rejection of malicious traffic does not establish resistance to all denial-of-service attacks.

Resource-exhaustion resistance requires measured performance evidence.

---

# 21. Information Disclosure Threats

Continuity and authorization integrity do not automatically provide confidentiality.

Potential disclosure risks include:

- Observable traffic metadata.
- Endpoint correlation.
- Session timing analysis.
- Error-message information leakage.
- Diagnostic data exposure.
- Sensitive information in evidence bundles.

## 21.1 Security requirements

Deployments requiring confidentiality must specify and verify the relevant cryptographic and operational protections.

## 21.2 Public evidence handling

Published logs and evidence must not expose:

- Secret keys.
- Authentication secrets.
- Confidential credentials.
- Protected implementation details.
- Sensitive deployment identifiers.

## 21.3 Limitation

This document does not claim universal traffic-analysis resistance, anonymity or metadata confidentiality.

---

# 22. Security Evidence Trust Boundary

Security evidence introduces a separate trust problem.

A test report may claim a PASS even when the underlying execution is incomplete, incorrectly attributed or altered.

## 22.1 Threats

- Modified logs.
- Incorrect source attribution.
- Missing negative tests.
- Suppressed failures.
- Unverifiable test commands.
- Incomplete evidence bundles.

## 22.2 Required evidence properties

- Source identity.
- Test identity.
- Execution context.
- Recorded verdict.
- Integrity verification.
- Explicit limitations.

## 22.3 Evidence acceptance

    SOURCE_IDENTIFIED = TRUE

    TEST_IDENTIFIED = TRUE

    EXECUTION_RECORDED = TRUE

    REQUIRED_POSTCONDITIONS_OBSERVED = TRUE

    EVIDENCE_INTEGRITY_VERIFIED = TRUE

A checksum does not independently prove the identity or trustworthiness of the evidence producer.

---

# 23. Threat-to-Test Traceability

The threat model should be linked to reproducible tests.

| Threat | Validation category |
|---|---|
| VRP-THR-001 | Endpoint adoption tests |
| VRP-THR-002 | Replay tests |
| VRP-THR-003 | Duplicate commit tests |
| VRP-THR-004 | Stale authority tests |
| VRP-THR-005 | Epoch rollback tests |
| VRP-THR-006 | Canonical-history integrity tests |
| VRP-THR-007 | Recovery authority tests |
| VRP-THR-008 | Independent session continuity tests |
| VRP-THR-009 | Concurrent admission tests |
| VRP-THR-010 | Malformed input tests |
| VRP-THR-011 | Resource pressure tests |
| VRP-THR-012 | Evidence verification tests |
| VRP-THR-013 | Application execution tests |
| VRP-THR-014 | Recovery under load tests |
| VRP-THR-015 | Authority separation tests |

A mapped test category is not proof of execution.

Every formal PASS requires attributable evidence.

---

# 24. Security Acceptance Gates

A security scenario must pass the required gates before receiving a formal PASS verdict.

## GATE 1 — Source Attribution

The tested implementation is identified.

## GATE 2 — Preconditions

The required initial security state is established.

## GATE 3 — Attack Execution

The adversarial input is demonstrably applied.

## GATE 4 — Admission Integrity

Invalid operations are rejected.

## GATE 5 — Authority Integrity

Unauthorized authority does not obtain execution rights.

## GATE 6 — Canonical Integrity

Protected state remains consistent with the security contract.

## GATE 7 — Positive Control

Required legitimate operations remain functional.

## GATE 8 — Evidence Integrity

The evidence record is attributable and verifiable.

## GATE 9 — Scope Declaration

The test limitations are explicitly documented.

### Verdict definitions

**PASS**

All required security postconditions are satisfied.

**FAIL**

At least one required security postcondition is violated.

**INCONCLUSIVE**

The evidence is insufficient to establish a verdict.

**NOT RUN**

The scenario has not been executed.

A missing required security observation must not be interpreted as a successful test.

---

# 25. Risk Assessment Framework

Threat severity should be assessed using explicit criteria.

The recommended dimensions are:

- Attack preconditions.
- Required attacker privileges.
- Exploitability.
- Impact on authority integrity.
- Impact on canonical state.
- Impact on availability.
- Exposure of the affected interface.
- Detection capability.
- Recovery consequences.

## 25.1 Critical

A demonstrated violation permits unauthorized protected execution, authority compromise or canonical-state corruption with severe impact.

## 25.2 High

A demonstrated violation significantly weakens an important security boundary.

## 25.3 Medium

A demonstrated violation creates a bounded security weakness requiring mitigation.

## 25.4 Low

A demonstrated issue has limited security impact under the evaluated assumptions.

## 25.5 Informational

A finding identifies an improvement or assurance limitation without a demonstrated security violation.

Risk ratings must be assigned to actual findings.

The existence of a threat in this document does not automatically establish a vulnerability or severity rating.

---

# 26. RED-to-GREEN Remediation Contract

Security findings should follow a reproducible remediation process.

    INSPECT
       |
       v
    DEFINE INVARIANT
       |
       v
    CONSTRUCT ADVERSARIAL TEST
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
    RUN REGRESSION
       |
       v
    VERIFY EVIDENCE
       |
       v
    CLOSE OR RETAIN FINDING

## 26.1 Required remediation record

    FINDING_ID

    THREAT_ID

    INVARIANT_ID

    RED_SOURCE_REVISION

    RED_EVIDENCE_REFERENCE

    CORRECTION_REVISION

    GREEN_EVIDENCE_REFERENCE

    REGRESSION_RESULT

    RESIDUAL_RISK

    CLOSURE_STATUS

## 26.2 Closure criteria

A security finding must not be considered closed solely because a test returns a zero exit code.

The original violation must be reproduced or otherwise convincingly established, the correction must be verified, and the relevant regression contract must remain satisfied.

---

# 27. Independent Security Review

Independent reviewers should be able to evaluate the declared security contract without requiring disclosure of proprietary algorithms.

A recommended review process is:

1. Select a threat from the register.
2. Identify the relevant trust boundary.
3. Identify the protected asset.
4. Determine the expected rejection behavior.
5. Obtain the associated public validation artifact.
6. Record the exact source revision.
7. Execute the relevant scenario.
8. Verify protected-state postconditions.
9. Confirm legitimate positive-control behavior.
10. Publish a scoped verdict.

Reviewers must distinguish between:

- Public reference-model evidence.
- Integrated runtime evidence.
- Private implementation evidence.
- Independent third-party evidence.

These evidence classes are not interchangeable.

---

# 28. Deployment-Specific Threat Extensions

The baseline model may be extended for particular deployment environments.

## 28.1 Mobile infrastructure

Relevant threats may include:

- Rapid endpoint changes.
- NAT rebinding.
- Radio access transitions.
- Relay disruption.
- Delayed traffic following handover.

## 28.2 Industrial networks

Relevant threats may include:

- Long-lived operational sessions.
- Intermittent links.
- Strict execution authorization.
- Recovery under constrained availability.
- Unauthorized command repetition.

## 28.3 Satellite-like communications

Relevant threats may include:

- High round-trip latency.
- Intermittent connectivity.
- Packet reordering.
- Extended blackout.
- Delayed replay attempts.

## 28.4 Distributed systems

Relevant threats may include:

- Conflicting authority.
- Concurrent mutation.
- Partial failures.
- State divergence.
- Recovery ordering.

These are candidate evaluation domains.

This document does not claim that VRP is certified or qualified for any particular industry or deployment.

---

# 29. Residual Risks

The following residual risks must remain explicit.

**RISK-001: Host compromise**

A fully compromised trusted host may invalidate assumptions outside the protocol's network-facing security boundary.

**RISK-002: Credential compromise**

Possession of valid compromised credentials may require additional revocation and incident-response controls.

**RISK-003: Implementation defects**

A correct public security contract does not guarantee defect-free implementation.

**RISK-004: Availability attacks**

Bounded resource behavior does not guarantee availability under unlimited attack traffic.

**RISK-005: Integration errors**

Application integration may violate assumptions established by the protocol-level model.

**RISK-006: Incomplete test coverage**

Finite adversarial testing cannot establish universal security.

**RISK-007: Evidence limitations**

Developer-produced evidence requires independent reproduction before it can be described as independently verified.

**RISK-008: Deployment variation**

Security behavior may depend on operating-system, network and configuration characteristics.

---

# 30. Public Assurance Position

VRP's public security assurance is based on:

- Explicit security invariants.
- Defined threat categories.
- Observable trust-boundary contracts.
- Adversarial test scenarios.
- Reproducible public validation artifacts.
- Evidence-based remediation.
- Explicit scope and limitations.

The public validation kit provides an entry point for examining selected reference-model behaviors.

Repository:

https://github.com/Endless33/vrp-validation-kit

The existence of a public validation artifact does not establish production qualification.

Integrated runtime results require separate evidence attribution.

Third-party verification must be identified as such and must not be inferred from internal testing.

---

# 31. Protected Core Disclosure Boundary

This document intentionally exposes:

- Public threat definitions.
- Security objectives.
- Trust-boundary responsibilities.
- Observable acceptance criteria.
- Adversarial test categories.
- Assurance limitations.

This document does not disclose:

- Proprietary authority-resolution algorithms.
- Private state-machine implementation.
- Internal continuity mechanisms.
- Protected runtime source code.
- Secret cryptographic material.
- Confidential packet-processing internals.
- Private deployment configurations.
- Undisclosed implementation-level security controls.

**PROTECTED CORE DISCLOSURE: NONE**

The purpose of this boundary is to enable meaningful external review of security behavior while preserving proprietary implementation details.

---

# 32. Related Documentation

## Security Assurance Case

    docs/review/VRP_SECURITY_ASSURANCE_CASE.md

## Adversarial Security Validation Report

    docs/security/VRP_ADVERSARIAL_SECURITY_VALIDATION_REPORT.md

## Evidence Traceability Matrix

    docs/review/VRP_EVIDENCE_TRACEABILITY_MATRIX.md

## Public Validation Kit

https://github.com/Endless33/vrp-validation-kit

All referenced paths and public artifacts should be checked against the published repository revision used for review.

---

# 33. Final Security Architecture Statement

The defining security challenge of a continuity-first protocol is not simply maintaining communication when the network changes.

It is preserving the legitimacy of execution while communication conditions change.

VRP's public security contract is expressed through the following principles:

**SESSION != TRANSPORT**

**TRANSPORT REACHABILITY != EXECUTION AUTHORITY**

**ENDPOINT CHANGE != AUTHORITY TRANSFER**

**REPLAY != NEW EXECUTION**

**REJECTION != PARTIAL COMMIT**

**RECOVERY != AUTHORITY RESET**

**TRANSPORT RESTORATION != PROVEN SESSION CONTINUITY**

**SECURITY CLAIM != VERIFIED SECURITY EVIDENCE**

The protocol must be evaluated not only when communication succeeds, but also when communication is manipulated, delayed, duplicated, interrupted or restored under adversarial conditions.

The architectural objective is:

**CONTINUITY WITHOUT AUTHORITY COMPROMISE.**

---

**END OF DOCUMENT**

**Veil Routing Protocol — VRP**

**Author:** Vitalijus Riabovas

© 2026 Vitalijus Riabovas. All rights reserved.

This document describes public security objectives, threat assumptions and verification contracts. It does not grant rights to proprietary implementation details. Applicable repository license terms govern reuse of repository content.
