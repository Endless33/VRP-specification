
# VRP ADVERSARIAL SECURITY VALIDATION REPORT

## Veil Routing Protocol (VRP)
### Adversarial Testing, Security Regression and Runtime Assurance

**Document ID:** VRP-ASVR-003  
**Version:** 1.0  
**Date:** 2026-11-12  
**Status:** Public Security Review Baseline  
**Classification:** PUBLIC — NON-PROPRIETARY  
**Author:** Vitalijus Riabovas  
**Project:** Veil Routing Protocol (VRP)

---

# 1. Executive Summary

The Veil Routing Protocol (VRP) is an experimental continuity-first networking protocol designed around a fundamental architectural distinction:

**SESSION != TRANSPORT**

The logical session and the underlying network transport are treated as separate architectural concerns.

Transport disruption must not automatically imply destruction of the logical session.

However, maintaining continuity is not sufficient to establish security.

A protocol must also demonstrate that continuity cannot be exploited to bypass authorization, replay protections, canonical-state integrity or authority boundaries.

VRP's adversarial validation program therefore evaluates two independent dimensions:

1. Whether legitimate continuity can be preserved under supported disruption conditions.
2. Whether unauthorized operations remain rejected during disruption, migration and recovery.

The security objective is not simply successful reconnection.

It is:

**CONTINUITY WITHOUT AUTHORITY COMPROMISE.**

This report documents the public security validation framework, the classes of adversarial testing undertaken during VRP development, the known public validation artifacts, and the evidence requirements for formal runtime qualification.

The report also defines the RED-to-GREEN remediation process used to distinguish reproducible security defects from verified corrections.

This document is not a third-party certification, penetration-testing certificate or production deployment authorization.

---

# 2. Scope of Assessment

The assessment framework addresses the following areas:

- Logical-session continuity.
- Transport-independent identity.
- Replay containment.
- Duplicate admission and commit rejection.
- Authority integrity.
- Epoch rollback rejection.
- Canonical-state preservation.
- Endpoint migration safety.
- Recovery behavior.
- Concurrent state mutation.
- Malformed traffic handling.
- Resource exhaustion resistance.
- Evidence integrity.
- Regression verification.

The assessment covers public reference-model testing and the evidence requirements for private runtime and integrated-system testing.

## 2.1 Assessment boundaries

The following components may be evaluated independently:

**Reference model**

An executable public implementation demonstrating selected protocol invariants.

**Protocol runtime**

An implementation enforcing protocol admission, authority and continuity contracts.

**Transport integration**

Network-facing execution involving real sockets, endpoints, operating-system behavior and transport disruption.

**Application integration**

End-to-end application behavior observed across disruption and recovery.

**Independent validation**

Reproduction by a reviewer outside the original development environment.

A passing result in one category must not be represented as proof of success in another.

---

# 3. Security Philosophy

VRP follows a fail-closed security philosophy.

A rejected operation must not obtain authority through an unintended side effect.

A malformed packet must not become an authorization event.

A stale authority must not regain control merely because the network path changes.

A duplicate operation must not create a second canonical commit.

A recovery procedure must not rewrite protected history.

A transport migration must not implicitly authorize a different logical identity.

These principles establish the distinction between availability and integrity.

Availability mechanisms must operate within the security contract.

They must not silently redefine it.

---

# 4. Core Security Invariants

The following invariant identifiers are aligned with the public evidence traceability framework.

| ID | Invariant | Security objective |
|---|---|---|
| VRP-INV-001 | Session Identity Continuity | Preserve logical identity across supported transport replacement |
| VRP-INV-002 | Transport Independence | Prevent endpoint identity from becoming canonical session identity |
| VRP-INV-003 | Replay Containment | Reject invalid duplicate or historical execution |
| VRP-INV-004 | Duplicate Commit Rejection | Prevent multiple canonical commits of one protected operation |
| VRP-INV-005 | Stale Authority Rejection | Prevent obsolete authority from mutating protected state |
| VRP-INV-006 | Epoch Rollback Rejection | Prevent invalid historical authority restoration |
| VRP-INV-007 | Canonical History Integrity | Prevent unauthorized history modification |
| VRP-INV-008 | Deterministic Authority Resolution | Resolve conflicting authority according to a defined contract |
| VRP-INV-009 | Recovery Integrity | Preserve security properties during recovery |
| VRP-INV-010 | Fail-Closed Admission | Reject unverified operations |
| VRP-INV-011 | Endpoint Adoption Safety | Prevent rejected traffic from authorizing path changes |
| VRP-INV-012 | Concurrent Admission Safety | Preserve admission invariants under concurrency |
| VRP-INV-013 | Evidence Integrity | Preserve attributable and tamper-evident results |
| VRP-INV-014 | Bounded Resource Behavior | Prevent uncontrolled resource consumption |
| VRP-INV-015 | Application Progress Preservation | Verify legitimate work can resume after supported recovery |

These invariants define intended security properties.

Their verification status depends on the corresponding test evidence.

---

# 5. Adversarial Threat Model

## 5.1 Attacker capabilities

The adversarial testing model considers an attacker who may be able to:

- Inject unexpected network traffic.
- Submit malformed protocol inputs.
- Replay previously observed packets.
- Duplicate traffic at high frequency.
- Reorder packets.
- Delay packet delivery.
- Trigger or exploit endpoint changes.
- Generate concurrent admission attempts.
- Submit stale or conflicting authority material.
- Apply sustained resource pressure.
- Exploit network instability during recovery.

These capabilities are scenario-specific assumptions.

They do not imply that every attacker can perform every operation in every deployment.

## 5.2 Protected assets

The principal protected assets include:

**Logical session identity**

The canonical identity of the continuing session.

**Authority integrity**

The validity of operations permitted to change protected state.

**Canonical history**

The accepted sequence of protected state transitions.

**Replay state**

The state used to distinguish valid execution from invalid repetition.

**Recovery integrity**

The security properties preserved through failure and restoration.

**Evidence integrity**

The authenticity and consistency of the recorded evaluation results.

## 5.3 Security boundaries

The following boundaries must remain explicit:

    UNTRUSTED NETWORK INPUT
               |
               v
       PROTOCOL ADMISSION
               |
               v
       AUTHORITY VALIDATION
               |
               v
       CANONICAL STATE
               |
               v
       AUTHORIZED EXECUTION

This diagram describes observable security responsibilities.

It is not a disclosure of the Protected Core implementation, internal algorithms or private execution topology.

---

# 6. Adversarial Test Environment

VRP development has included adversarial testing and security regression work in controlled Linux environments.

The engineering environments used across the project include:

- Oracle Linux virtual machines.
- VMware-based test infrastructure.
- Go runtime test harnesses.
- Kali Linux security testing.
- Isolated validation repositories.
- Public reproducible reference-model tests.

## 6.1 Evidence requirements

A formal public test record should include:

| Property | Required evidence |
|---|---|
| Operating system | Distribution and version |
| Runtime | Exact version |
| Source identity | Commit and tree identifiers |
| Test topology | Relevant network and process boundaries |
| Attack scenario | Stable identifier |
| Attack configuration | Recorded parameters |
| Execution | Captured command and exit status |
| Observations | Logs and measured results |
| Integrity | Checksums and applicable signatures |
| Verdict | Explicit PASS, FAIL or INCONCLUSIVE |
| Limitations | Scope and exclusions |

Historical test environments should not be assigned exact versions retrospectively without supporting records.

---

# 7. Kali Linux Adversarial Testing

Kali Linux is used as a security testing environment for controlled adversarial evaluation.

The relevant security question is not whether a particular penetration-testing distribution was used.

The question is whether a reproducible attack stimulus caused a violation of a defined protocol invariant.

The public validation register separates the following attack classes.

## 7.1 Malformed Traffic

**Campaign ID:** VRP-KALI-001

**Target invariant:** VRP-INV-010

### Attack categories

- Unexpected packet structure.
- Invalid packet lengths.
- Truncated input.
- Unknown protocol fields.
- Unsupported message types.
- Invalid or inconsistent metadata.
- Unexpected state-transition requests.

### Required behavior

Invalid input must not acquire execution authority.

Malformed traffic must not cause unauthorized canonical-state mutation.

Unexpected inputs must be handled without violating the applicable runtime safety contract.

### Required evidence

- Input classification.
- Admission decision.
- Runtime error or rejection classification.
- Canonical-state observation.
- Process stability.
- Resource consumption.

### Public evidence status

**HISTORICAL TEST CATEGORY — FORMAL PUBLIC EVIDENCE PENDING**

No universal malformed-input security claim is made without attributable test records.

---

## 7.2 Replay and Duplicate Traffic

**Campaign ID:** VRP-KALI-002

**Target invariants:**

VRP-INV-003  
VRP-INV-004  
VRP-INV-012

### Attack categories

- Repeated sequence submission.
- Duplicate packet injection.
- Historical sequence replay.
- Concurrent duplicate admission.
- Replay pressure during recovery.

### Required behavior

Duplicate execution must not create unauthorized canonical progress.

The same protected operation must not be committed twice.

Concurrent admission must preserve the same security contract as sequential admission.

### Required evidence

- Submitted operation count.
- Accepted operation count.
- Rejected operation count.
- Duplicate commit count.
- Canonical state before and after.
- Legitimate positive-control result.

### Public evidence status

**REFERENCE-MODEL COVERAGE AVAILABLE**

Integrated runtime results require separately attributable evidence.

---

## 7.3 Endpoint and Transport Manipulation

**Campaign ID:** VRP-KALI-003

**Target invariants:**

VRP-INV-001  
VRP-INV-002  
VRP-INV-011

### Attack categories

- Unexpected source endpoint changes.
- NAT rebinding simulation.
- Candidate path manipulation.
- Rejected path adoption attempts.
- Transport replacement under disruption.

### Required behavior

A new endpoint must not automatically become authorized.

Rejected input must not create a persistent transport-authority change.

Legitimate migration must preserve the applicable logical-session identity contract.

### Required evidence

- Authorized endpoint before testing.
- Candidate endpoint.
- Admission decision.
- Authorized endpoint after testing.
- Independent session-identity observation.
- Valid application traffic after recovery.

### Public evidence status

**RUNTIME EVIDENCE PENDING PUBLIC ATTRIBUTION**

---

## 7.4 Resource Pressure

**Campaign ID:** VRP-KALI-004

**Target invariant:** VRP-INV-014

### Attack categories

- High-rate invalid traffic.
- Sustained replay pressure.
- Concurrent admission bursts.
- Repeated malformed inputs.
- Recovery under traffic pressure.

### Required behavior

The implementation must respect documented resource bounds under the evaluated conditions.

Failure handling must not silently bypass admission or authority requirements.

### Required evidence

- Traffic rate.
- Test duration.
- Memory consumption.
- CPU consumption.
- Queue or buffer utilization where available.
- Admission outcomes.
- Recovery behavior.
- Explicit operating limits.

### Public evidence status

**QUALIFICATION REQUIRES MEASURED RESOURCE DATA**

A successful functional rejection test is not equivalent to denial-of-service resistance.

---

## 7.5 Compound Network Disruption

**Campaign ID:** VRP-KALI-005

**Target invariants:**

VRP-INV-001  
VRP-INV-003  
VRP-INV-005  
VRP-INV-009  
VRP-INV-015

### Attack categories

- Endpoint churn combined with packet loss.
- Blackout combined with replay.
- Delayed packets following recovery.
- Competing authority during restoration.
- High-latency and reordered delivery.

### Required behavior

Supported recovery must preserve canonical session identity and authority integrity.

Invalid operations must remain rejected during the recovery window.

Successful transport restoration alone is insufficient evidence of session continuity.

### Public evidence status

**COMPOUND VALIDATION PROFILE — RESULTS REQUIRE INDIVIDUAL EVIDENCE**

---

# 8. Public Replay Storm Validation

## 8.1 Scenario

**Reference ID:** VRP-PUB-REPLAY-9999

**Category:** Replay containment.

**Evidence class:** Public reference-model evaluation.

**Repository:**

https://github.com/Endless33/vrp-validation-kit

The public validation kit includes a replay-storm scenario intended to demonstrate the rejection of 9,999 duplicate sequence attempts.

## 8.2 Expected behavior

    DUPLICATE_ATTEMPTS = 9999

    EXPECTED_DUPLICATE_ACCEPTED = 0

    EXPECTED_DUPLICATE_REJECTED = 9999

    EXPECTED_CANONICAL_STATE = PRESERVED

The next valid sequence must remain admissible according to the reference-model contract.

## 8.3 Reproduction

    git clone https://github.com/Endless33/vrp-validation-kit.git

    cd vrp-validation-kit

    git rev-parse HEAD

    git rev-parse 'HEAD^{tree}'

    go run ./cmd/vrp-replay-storm-9999

The executable path must be checked against the selected repository revision before the command is treated as an authoritative reproduction procedure.

## 8.4 Interpretation

A passing reference-model execution demonstrates the behavior of the evaluated model.

It does not establish:

- Kernel-level replay rejection.
- Production packet-path enforcement.
- End-to-end mobile network security.
- Immunity to all replay variants.
- Formal verification of the Protected Core.
- Independent certification.

**Assurance classification:**

PUBLIC REFERENCE-MODEL VALIDATION

---

# 9. Authority Integrity Testing

Authority integrity is a separate security requirement from transport continuity.

A protocol may preserve connectivity while incorrectly accepting obsolete or conflicting authority.

Such behavior must be treated as a security failure.

## 9.1 Stale authority

**Test ID:** VRP-TST-005

### Required behavior

Obsolete authority must not modify protected state.

### Required postconditions

    STALE_AUTHORITY_ACCEPTED = FALSE

    UNAUTHORIZED_COMMIT = FALSE

    CANONICAL_HISTORY_PRESERVED = TRUE

## 9.2 Epoch rollback

**Test ID:** VRP-TST-006

### Required behavior

Invalid historical epochs must not regain authority.

### Required postconditions

    INVALID_ROLLBACK_ACCEPTED = FALSE

    CANONICAL_AUTHORITY_PRESERVED = TRUE

## 9.3 Competing authority

**Test ID:** VRP-TST-008

### Required behavior

Conflicting authority must be handled according to a deterministic, testable contract.

The evaluator must establish whether the outcome is repeatable under the same declared conditions.

### Required evidence

- Initial authority state.
- Competing inputs.
- Final authority state.
- Accepted and rejected transitions.
- Repeatability observations.

**Status:**

FORMAL PUBLIC RUNTIME VERDICTS REQUIRE ATTRIBUTABLE EVIDENCE

---

# 10. Canonical History Protection

**Test ID:** VRP-TST-007

The canonical history represents accepted protected state transitions.

A rejected operation must not be capable of rewriting previously accepted canonical history.

## 10.1 Adversarial scenario

An unauthorized input attempts to cause a protected historical mutation.

## 10.2 Required observations

Before the attempted mutation:

    CANONICAL_STATE = BASELINE

After the rejected operation:

    CANONICAL_STATE = BASELINE

The comparison must cover the relevant protected state rather than relying exclusively on a human-readable success message.

## 10.3 Acceptance criteria

- Unauthorized operation rejected.
- No unauthorized canonical mutation.
- No unintended authority transfer.
- Evidence sufficient to compare preconditions and postconditions.

---

# 11. Concurrency and Linearizability

Concurrency introduces attack surfaces that sequential tests may not detect.

A replay admission mechanism may behave correctly in isolation but violate its contract when multiple execution contexts operate concurrently.

## 11.1 Threat categories

- Simultaneous duplicate submission.
- Concurrent acceptance attempts.
- Interleaved replay and legitimate traffic.
- Admission during state transition.
- Concurrent recovery-related operations.

## 11.2 Required invariant

For a protected operation that permits a single successful admission:

    ACCEPTED_DUPLICATES <= 1

A stronger zero-duplicate acceptance requirement applies when the operation has already been admitted.

## 11.3 Verification methods

Relevant methods include:

- Concurrent regression tests.
- Race detection.
- Repeated adversarial scheduling.
- State-transition consistency checks.
- Acceptance-count verification.

Race-detector success alone does not prove linearizability.

Linearizability requires verification of the operation's observable atomicity contract.

## 11.4 Required evidence

- Source revision.
- Concurrency configuration.
- Operation count.
- Accepted count.
- Rejected count.
- Race-detector result.
- Repetition count.
- Canonical-state observations.

---

# 12. RED-to-GREEN Security Remediation

Security remediation must preserve evidence of the original defect.

A passing regression test without a reproducible original failure provides weaker assurance than a complete remediation chain.

VRP's remediation reporting model is:

    INSPECT
       |
       v
    HYPOTHESIS
       |
       v
    RED
       |
       v
    PROVE FAILURE
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
    EVIDENCE REVIEW

## 12.1 RED phase

A failing test must demonstrate an actual violation of the intended contract.

A compile error, invalid test fixture or unrelated environment failure must not automatically be classified as a security vulnerability.

The failure must be attributable.

## 12.2 Correction phase

A security correction should be limited to the necessary behavior.

The correction must not weaken the original invariant simply to obtain a passing test.

## 12.3 GREEN phase

The original failing scenario must be executed again against the corrected revision.

The relevant security postconditions must be observed.

## 12.4 Regression phase

Related legitimate behavior must remain functional.

The correction must not silently introduce an unrelated availability or integrity regression.

## 12.5 Evidence requirements

Every published RED-to-GREEN record should identify:

    FINDING_ID

    INVARIANT_ID

    RED_SOURCE_REVISION

    RED_TEST_ID

    RED_EVIDENCE_REFERENCE

    CORRECTION_REVISION

    GREEN_TEST_ID

    GREEN_EVIDENCE_REFERENCE

    REGRESSION_RESULT

    RESIDUAL_LIMITATIONS

This format supports external audit without disclosing proprietary correction algorithms.

---

# 13. Runtime Recovery and Blackout Testing

**Test ID:** VRP-TST-009

Recovery must be evaluated using explicit observations.

A restored UDP socket is not equivalent to a preserved logical session.

A surviving process identifier is not proof of logical continuity.

A successful application packet does not independently establish authority integrity.

## 13.1 Required measurements

- Logical-session identity before disruption.
- Transport identity before disruption.
- Disruption start time.
- Disruption end time.
- Recovery completion time.
- Logical-session identity after recovery.
- Authority state after recovery.
- Application delivery after recovery.

## 13.2 Proposed blackout profiles

| Profile | Blackout duration |
|---|---|
| BL-10 | 10 seconds |
| BL-30 | 30 seconds |
| BL-60 | 60 seconds |

## 13.3 Required verdict

**PASS**

The independent session oracle confirms the required identity continuity and all applicable security postconditions hold.

**FAIL**

The logical session changes unexpectedly or a protected invariant is violated.

**INCONCLUSIVE**

The test demonstrates transport restoration without sufficient evidence of logical-session continuity.

These profiles are test definitions, not claims of completed qualification.

---

# 14. High-Latency and Loss Testing

VRP's continuity model is relevant to unstable network environments.

Examples include:

- Mobile network transitions.
- Intermittent wireless connectivity.
- Satellite-like communication conditions.
- Relay disruption.
- High-loss network paths.

## 14.1 Proposed loss profiles

    LOSS_30_PERCENT

    LOSS_50_PERCENT

    LOSS_80_PERCENT

    LOSS_90_PERCENT

## 14.2 Proposed latency profile

    RTT = 300 TO 900 MS

    JITTER = ENABLED

    REORDERING = ENABLED

## 14.3 Required measurements

- Delivered application operations.
- Lost operations.
- Duplicate delivery.
- Recovery time.
- Session identity.
- Authority integrity.
- Replay rejection.
- Resource consumption.

A successful test at one packet-loss rate does not establish success at a higher rate.

Results must be reported separately.

---

# 15. Independent Session Identity Verification

The fundamental VRP continuity claim requires independent verification.

**SESSION != TRANSPORT**

The identity oracle must observe the logical session independently of the transport mechanism being disrupted.

## 15.1 Invalid proof substitutes

The following observations are insufficient by themselves:

- Process remains alive.
- Socket is recreated.
- UDP traffic resumes.
- Tunnel reconnects.
- Endpoint address changes.
- Application receives a packet.

These observations may be relevant supporting evidence.

They are not substitutes for logical-session identity verification.

## 15.2 Required verification tuple

    LOGICAL_SESSION_IDENTITY

    TRANSPORT_IDENTITY

    AUTHORITY_STATE

    CANONICAL_STATE

    APPLICATION_PROGRESS

    RECOVERY_TIME

## 15.3 Acceptance criteria

The test must establish that the same logical session persists across a supported transport transition.

The applicable authority and canonical-state invariants must remain satisfied.

The evaluator must also distinguish preserved session identity from recreation of an equivalent-looking session.

---

# 16. Evidence Integrity and Provenance

A security result is only as useful as the evidence supporting it.

## 16.1 Source attribution

Every formal runtime result should identify:

- Repository or controlled source identifier.
- Commit identifier.
- Tree identifier where applicable.
- Build information.
- Runtime version.
- Test environment.
- Test command.
- Exit status.

## 16.2 Evidence integrity

Finalized evidence bundles should include checksums.

Where cryptographic signatures are used, the verification mechanism and trust assumptions must be documented.

A checksum alone establishes consistency with a reference digest.

It does not establish who produced the artifact.

## 16.3 Immutable evidence

Finalized evidence should not be silently modified.

Any correction to a published record should create an attributable revision or replacement record.

## 16.4 Recommended structure

    evidence/
      campaign-id/
        MANIFEST.md
        BUILD_INFO.txt
        SOURCE_IDENTITY.txt
        TEST_CONFIGURATION.json
        EXECUTION.log
        OBSERVATIONS.json
        VERDICT.md
        SHA256SUMS

Sensitive data must be removed or appropriately redacted before publication.

---

# 17. Security Verification Gates

A formal adversarial test verdict should satisfy the following gates.

## GATE 1 — Source Identity

The evaluated implementation is identified.

## GATE 2 — Preconditions

The required initial state is established.

## GATE 3 — Attack Execution

The intended adversarial stimulus is demonstrably applied.

## GATE 4 — Rejection Contract

Prohibited behavior is rejected.

## GATE 5 — Canonical State

No unauthorized protected-state mutation occurs.

## GATE 6 — Positive Control

Legitimate behavior remains functional where required.

## GATE 7 — Evidence Integrity

The evidence record is attributable and internally consistent.

## GATE 8 — Scope Declaration

The verdict identifies what was and was not tested.

### Verdict policy

    ALL_REQUIRED_GATES_PASS
        -> PASS

    REQUIRED_SECURITY_CONDITION_VIOLATED
        -> FAIL

    EVIDENCE_INSUFFICIENT
        -> INCONCLUSIVE

    TEST_NOT_EXECUTED
        -> NOT RUN

A missing required gate must not be silently treated as successful.

---

# 18. Public Assurance Status

The following table distinguishes documented public validation surfaces from runtime evidence requiring additional publication.

| Assessment area | Public evidence classification |
|---|---|
| Protocol invariant definitions | Public specification |
| Replay storm scenario | Public reference-model validation |
| Public attack scenarios | Public validation kit |
| Evidence verification tooling | Public validation tooling |
| Runtime replay linearizability | Runtime evidence attribution required |
| Authority epoch integrity | Runtime evidence attribution required |
| Kali Linux malformed traffic | Campaign evidence publication required |
| Endpoint adoption safety | Runtime evidence attribution required |
| Independent session continuity | Session-oracle evidence required |
| Compound loss and blackout | Scenario-specific evidence required |
| Production deployment qualification | Not established by this report |
| Third-party certification | Not claimed |

This table does not downgrade internally executed tests.

It prevents internal development results from being presented as independently reproduced public evidence without the necessary supporting artifacts.

---

# 19. Residual Risks and Limitations

The following limitations remain relevant to any public security assessment.

## 19.1 Incomplete attack coverage

No finite adversarial test suite can establish resistance to every possible attack.

## 19.2 Environment dependence

Results obtained in virtualized Linux environments may not transfer directly to all operating systems, network devices or production deployments.

## 19.3 Resource limits

Functional rejection does not establish resistance to unbounded denial-of-service pressure.

## 19.4 Independent verification

Developer-generated evidence is not equivalent to independent third-party certification.

## 19.5 Transport diversity

Successful continuity across one transport transition does not establish equivalent behavior across every possible transport.

## 19.6 Operational integration

Protocol-level correctness does not automatically establish deployment suitability for carrier networks, industrial control systems, satellite systems or critical infrastructure.

## 19.7 Protected implementation

Public reviewers may verify observable contracts without access to the proprietary implementation.

Such verification must be described according to its actual scope.

---

# 20. Recommended Independent Evaluation

An independent reviewer should:

1. Select an invariant from the public register.
2. Identify its associated test scenario.
3. Obtain the corresponding public artifact.
4. Record the exact source revision.
5. Execute the documented reproduction procedure.
6. Verify the expected negative behavior.
7. Verify the required positive control.
8. Inspect canonical-state observations.
9. Verify evidence integrity.
10. Publish a scoped verdict with limitations.

For non-public runtime evaluations, access and disclosure conditions must be agreed separately.

No reviewer should be required to accept a security claim solely because the project author reports it.

Conversely, lack of access to proprietary algorithms does not prevent meaningful black-box evaluation of published behavioral contracts.

---

# 21. Security Assurance Conclusions

VRP's adversarial security program is organized around a central requirement:

**A CONTINUING SESSION MUST REMAIN A SECURE SESSION.**

Transport resilience must not create an authorization bypass.

Recovery must not permit canonical-history corruption.

Replay containment must remain effective during concurrent execution.

Endpoint migration must not grant authority to rejected traffic.

Security corrections must be supported by reproducible RED-to-GREEN evidence.

Public reference-model results must remain distinguishable from integrated runtime qualification.

The objective is not to claim that all possible failure conditions have been eliminated.

The objective is to make each security claim measurable, attributable and independently reviewable.

---

# 22. Public References

## VRP Specification

https://github.com/Endless33/Veil-Routing-Protocol

## VRP Validation Kit

https://github.com/Endless33/vrp-validation-kit

## Related Documents

Security Assurance Case:

    docs/review/VRP_SECURITY_ASSURANCE_CASE.md

Evidence Traceability Matrix:

    docs/review/VRP_EVIDENCE_TRACEABILITY_MATRIX.md

---

# 23. Protected Core Disclosure Boundary

This report intentionally documents:

- Public security invariants.
- Adversarial testing categories.
- Expected runtime behavior.
- Evidence requirements.
- Verification methodology.
- Security remediation discipline.
- Explicit assurance limitations.

It does not disclose:

- Proprietary protocol algorithms.
- Internal authority-resolution mechanisms.
- Private state-transition logic.
- Protected runtime implementation.
- Secret material.
- Internal cryptographic key handling.
- Non-public exploit or bypass details.
- Confidential deployment configurations.

**PROTECTED CORE DISCLOSURE: NONE**

Public validation is based on observable behavior and independently checkable evidence.

---

# 24. Final Engineering Statement

A security architecture should not be judged solely by how it behaves when the network is healthy.

It must also be evaluated when:

- Packets are duplicated.
- Endpoints change.
- Authority becomes stale.
- Traffic arrives out of order.
- Recovery is interrupted.
- Malformed input reaches admission boundaries.
- Concurrent operations compete.
- The transport disappears.

For VRP, the intended engineering contract is:

**SESSION != TRANSPORT**

**TRANSPORT REACHABILITY != EXECUTION AUTHORITY**

**RECONNECTION != SESSION CONTINUITY**

**REJECTION != PARTIAL COMMIT**

**RECOVERY != AUTHORITY RESET**

**TEST PASS != UNIVERSAL SECURITY PROOF**

**SECURITY CLAIMS REQUIRE EVIDENCE.**

---

**END OF DOCUMENT**

**Veil Routing Protocol — VRP**

**Author:** Vitalijus Riabovas

© 2026 Vitalijus Riabovas. All rights reserved.

This public document describes security objectives, verification methods and bounded assurance claims. It grants no additional rights to proprietary implementation details. Applicable repository license terms govern reuse of repository content.
