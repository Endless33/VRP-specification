
# VRP SECURITY ASSURANCE CASE

## Veil Routing Protocol (VRP)
### Public Security Claims, Evidence Requirements and Verification Boundaries

**Document ID:** VRP-SAC-001  
**Version:** 1.0  
**Status:** Public Review Baseline  
**Classification:** PUBLIC — NON-PROPRIETARY  
**Protocol:** Veil Routing Protocol (VRP)  
**Author and Protocol Architect:** Vitalijus Riabovas  
**Project:** Jumping VPN / VRP  
**Date:** 2026-11-12

---

## 1. Executive Statement

Veil Routing Protocol (VRP) is a continuity-first protocol and runtime architecture designed to separate logical session identity from the underlying network transport.

Its foundational architectural principle is:

**SESSION != TRANSPORT**

A network connection may disappear, migrate, become unavailable or be replaced without automatically requiring the logical session to be reconstructed.

However, maintaining connectivity is not sufficient to establish correctness.

A resilient system must also preserve valid authority, canonical state, replay boundaries and execution integrity.

VRP therefore treats session continuity as a security-sensitive correctness property rather than a simple reconnection feature.

This document establishes the public assurance framework against which VRP implementations and validation results should be evaluated.

It defines:

- Security and continuity claims.
- Required observable properties.
- Adversarial validation requirements.
- Evidence acceptance criteria.
- Failure and recovery expectations.
- Independent verification boundaries.
- Conditions that invalidate assurance claims.

The purpose is to establish an auditable engineering contract without disclosing protected runtime implementation details.

---

## 2. Assurance Objective

The primary assurance objective is:

> A validated VRP implementation shall preserve authorized logical-session continuity across supported transport disruptions without allowing unauthorized, stale, replayed or conflicting execution to corrupt canonical state.

This objective contains three separate obligations.

### 2.1 Continuity

Permitted transport changes must not automatically invalidate an otherwise valid logical session.

### 2.2 Security

Continuity mechanisms must not bypass admission, authorization, replay protection or canonical-state integrity.

### 2.3 Verifiability

Security and continuity claims must be supported by observable, reproducible and attributable evidence.

No single successful connection, recovery event or demonstration is sufficient to establish all three obligations.

---

## 3. Assurance Principles

### SAC-01 — Logical Session Independence

A logical session shall not be identified exclusively by:

- A transport socket.
- A source IP address.
- A destination IP address.
- A UDP source port.
- A network interface.
- A process identifier.
- A specific relay.
- A current network route.

A successful transport reconnection alone shall not constitute evidence of logical-session continuity.

**Verification requirement:**

The evaluator must observe stable logical-session identity across a controlled transport replacement using a session-identity oracle independent of transport and process identity.

**Failure condition:**

A replacement transport is incorrectly reported as the original logical session without verifiable identity continuity.

---

### SAC-02 — Canonical Authority Integrity

Network reachability shall not automatically establish execution authority.

The following distinctions are fundamental:

**REACHABILITY != AUTHORITY**

**PACKET OBSERVATION != PACKET ADMISSION**

**CANDIDATE PATH != AUTHORIZED PATH**

A transport endpoint may be observed without being authorized to modify persistent session state.

**Verification requirement:**

Adversarial tests must demonstrate that rejected or unauthorized inputs cannot obtain persistent authority over canonical execution or transport-path selection.

**Failure condition:**

Rejected traffic changes canonical authority or persistent active-path state.

---

### SAC-03 — Replay Containment

Previously accepted, duplicated or otherwise invalid historical work shall not be admitted as new authorized execution.

A valid replay-rejection result requires more than a rejection counter.

The evaluator must establish that:

1. The prohibited replay was rejected.
2. The protected canonical state remained unchanged.
3. The rejection did not create unauthorized authority.
4. Subsequent legitimate progress remained possible.

**Required invariant:**

REJECTED REPLAY  
=> NO UNAUTHORIZED CANONICAL MUTATION

**Positive control:**

Following replay pressure, the next valid operation must remain admissible when all applicable security conditions are satisfied.

**Failure condition:**

A replay is admitted, canonical state is corrupted, or legitimate execution becomes permanently blocked as an unintended consequence of replay handling.

---

### SAC-04 — Epoch and Lifecycle Safety

Historical execution contexts must not silently regain current authority.

A qualified implementation must reject:

- Stale authority.
- Unauthorized epoch rollback.
- Invalid historical execution.
- Duplicate commits.
- Unauthorized lifecycle resurrection.

**Verification requirement:**

Negative tests must exercise stale and conflicting execution contexts.

**Failure condition:**

An obsolete authority context successfully changes protected canonical state.

---

### SAC-05 — Canonical State Preservation

Rejected adversarial input shall not produce an unauthorized persistent state transition.

This applies to security-relevant effects including:

- Session lifecycle.
- Canonical execution history.
- Authorized transport-path selection.
- Authority ownership.
- Commit acceptance.
- Recovery state.

**Required invariant:**

REJECTION != PARTIAL COMMIT

**Verification requirement:**

State must be evaluated before and after the rejected operation through an authorized observation surface.

**Failure condition:**

A rejected operation leaves unauthorized persistent state changes.

---

### SAC-06 — Fail-Closed Security Decisions

When the runtime cannot establish the validity of an operation, it shall not grant additional authority merely to preserve connectivity.

An unknown or ambiguous input must not automatically become trusted.

Examples include:

- Malformed protocol input.
- Unsupported message types.
- Invalid authentication context.
- Replay ambiguity.
- Conflicting authority claims.
- Unrecognized migration requests.
- Inconsistent recovery state.
- Invalid session-lifecycle references.

A fail-closed decision may intentionally reduce availability.

Such behavior must not be misrepresented as uninterrupted service.

**Required principle:**

CORRECTNESS BEFORE CONNECTIVITY

**Failure condition:**

Unverified input receives privileged admission because the runtime attempts to maintain apparent availability.

---

### SAC-07 — Recovery Integrity

A recovery procedure must not silently:

- Create unauthorized session identity.
- Reintroduce stale authority.
- Accept duplicate canonical work.
- Rewind protected lifecycle state.
- Convert rejected traffic into authorized state.
- Report recovery success without verifying the applicable postconditions.

Recovery must be evaluated independently from basic transport reconnection.

**Verification requirement:**

Recovery reports must distinguish:

1. Transport availability.
2. Logical-session identity.
3. Authority validity.
4. Canonical-state integrity.
5. Application-level progress.

**Failure condition:**

A recovered connection is reported as a recovered logical session while required integrity properties are unverified or violated.

---

## 4. Threat and Failure Coverage

The following categories form the baseline adversarial evaluation scope.

| Threat / failure | Required observation |
|---|---|
| Packet loss | Admission correctness and delivery outcome |
| Reordering | Canonical ordering and replay handling |
| Packet duplication | Duplicate-admission rejection |
| Replay storm | State integrity and legitimate progress |
| NAT rebinding | Unauthorized path adoption prevention |
| Endpoint churn | Logical-session identity across path changes |
| Relay failure | Controlled recovery and authority integrity |
| Network blackout | Recovery behavior and measured interruption |
| Conflicting authority | Deterministic, authorized resolution |
| Epoch rollback | Rejection of invalid historical authority |
| Malformed traffic | Safe rejection without persistent corruption |
| Unknown protocol input | Fail-closed decision and bounded resource use |
| Concurrent mutation | State consistency under contention |
| Resource exhaustion | Bounded degradation and recovery behavior |
| Compound failures | Interaction effects across multiple disruptions |

These categories represent evaluation requirements, not a declaration that every combination has already passed.

Each claim must be qualified by its actual test environment, implementation revision and recorded result.

---

## 5. Required Evidence Hierarchy

VRP evaluations shall distinguish the following evidence classes.

### E0 — Specification or Contract

A published statement defining intended behavior.

Examples:

- Session independence.
- Replay rejection.
- Canonical-state integrity.

This establishes requirements.

It does not establish implementation correctness.

### E1 — Public Reference Model

A reproducible public model demonstrating a declared invariant.

This may validate the model's behavior without exercising the protected runtime.

### E2 — Controlled Runtime Evaluation

An instrumented or black-box evaluation of an identified runtime revision under controlled conditions.

The evaluated implementation and test environment must be attributable.

### E3 — Integrated System Evaluation

A test involving the relevant transport, runtime, operating system and application behavior.

End-to-end conclusions require end-to-end observations.

### E4 — Independently Reproduced Evaluation

A qualified external evaluator reproduces a scoped test against an identifiable artifact.

A separate verification program does not, by itself, establish independent third-party review.

### E5 — Operational Qualification

Evidence collected against explicitly defined deployment requirements, operational conditions and acceptance criteria.

Operational qualification must not be inferred from laboratory success.

**Evidence levels are not interchangeable.**

A public reference-model PASS cannot be promoted into an integrated deployment PASS.

---

## 6. Public Replay Validation Reference

The VRP External Validation Kit includes a standalone replay-invariant evaluation using 9,999 duplicate-sequence attempts.

Repository:

https://github.com/Endless33/vrp-validation-kit

The public evaluator can reproduce the relevant model using:

    git clone https://github.com/Endless33/vrp-validation-kit.git

    cd vrp-validation-kit

    git rev-parse HEAD
    git rev-parse 'HEAD^{tree}'
    git status --short

    go run ./cmd/vrp-replay-storm-9999

The expected model-level postconditions include:

- All 9,999 duplicate presentations rejected.
- Zero duplicate presentations accepted.
- No unauthorized canonical sequence advancement.
- Successful admission of the next valid sequence.

The public validator is a standalone invariant model.

It must not be described as proof that 9,999 real network packets traversed a protected runtime unless a separate runtime-specific record establishes that fact.

The result shall retain its declared execution scope.

---

## 7. Adversarial Test Evidence Requirements

Every published adversarial result should include the following fields.

| Field | Requirement |
|---|---|
| Test ID | Stable, unique identifier |
| Claim ID | Security obligation being evaluated |
| Source commit | Exact commit SHA |
| Source tree | Exact Git tree SHA |
| Artifact identity | Digest or immutable artifact reference |
| Runtime version | Identifiable evaluated build |
| Environment | OS, kernel, runtime and tool versions |
| Attack input | Reproducible stimulus description |
| Test parameters | Exact relevant values and durations |
| Expected outcome | Defined before evaluation |
| Observed outcome | Captured execution result |
| Positive control | Valid operation after adversarial input |
| Negative control | Defined prohibited operation |
| Evidence | Immutable or integrity-checked artifacts |
| Limitations | Exclusions and untested conditions |
| Verdict | PASS, FAIL, INCONCLUSIVE or NOT RUN |

A verdict must not be accepted without sufficient evidence to reconstruct its meaning.

---

## 8. Kali Linux and Red-Team Qualification

Security evaluations performed using Kali Linux or other adversarial tooling shall be published with explicit scope and evidence.

At minimum, the report should identify:

- Kali Linux version.
- Tool versions.
- Evaluated implementation revision.
- Network topology.
- Target environment.
- Attack class.
- Input parameters.
- Duration.
- Authorized test boundaries.
- Observable runtime response.
- Captured evidence.
- Known limitations.

A successful red-team evaluation is evidence for the tested scenario.

It is not proof of universal attack resistance.

### 8.1 RED-to-GREEN Remediation

Where a defect has been discovered and corrected, the public remediation record should establish:

**RED:**

A reproducible test demonstrates a violation of the intended contract.

**FIX:**

A scoped implementation change addresses the demonstrated violation.

**GREEN:**

The original test passes against the corrected implementation.

**REGRESSION:**

Additional tests evaluate whether the change introduced unintended behavior.

**INTEGRITY:**

The evidence identifies the applicable source revision and artifacts.

A RED-to-GREEN report should not imply that all security defects have been eliminated.

Details of protected implementation mechanisms shall remain private.

---

## 9. Independent Session-Identity Verification

The following observations are insufficient by themselves:

- The same PID remains alive.
- A UDP socket is recreated.
- A new connection succeeds.
- Traffic resumes.
- A VPN interface remains visible.
- A client process reconnects.

A continuity claim requires evidence of the logical-session identity itself.

The required test structure is:

**BASELINE**

Observe the established logical-session identity.

**DISRUPTION**

Introduce a controlled transport failure or replacement.

**RECOVERY**

Restore an authorized communication path.

**VERIFICATION**

Use an independent observation mechanism to determine whether the original logical session remains valid.

The evaluator must separately record:

- Session identity.
- Transport identity.
- Authority validity.
- Canonical state.
- Application progress.
- Recovery time.

If a session-identity oracle is unavailable, the continuity result must be marked INCONCLUSIVE rather than inferred from connectivity.

---

## 10. Unknown Input and Defensive Behavior

An enterprise deployment may encounter events that were not represented in the original test campaign.

Examples include:

- Previously unseen packet structures.
- Unexpected endpoint behavior.
- Invalid state transitions.
- Conflicting recovery events.
- Resource exhaustion.
- Compound network failures.

The defensive contract requires that unknown input must not gain authorization by default.

The evaluator shall examine whether the system:

1. Rejects unauthorized input.
2. Preserves protected state.
3. Avoids unintended privilege escalation.
4. Maintains bounded resource consumption.
5. Produces an attributable decision record.
6. Permits legitimate recovery when safe.
7. Refuses recovery when required integrity conditions cannot be established.

This is a contract for bounded defensive decision-making.

It is not a claim of artificial intelligence, unrestricted autonomous response or universal threat recognition.

---

## 11. Evidence Integrity and Provenance

A security result must be attributable to the artifact that produced it.

Published evidence should provide:

- An immutable source reference.
- A source-tree identity.
- Test configuration.
- Execution environment.
- Captured output.
- Exit status.
- Artifact digests.
- Verification procedure.
- Scope limitations.

Where signatures are available, reviewers should verify them against an explicitly trusted key identity.

A hash establishes integrity relative to a trusted reference.

It does not independently establish that the execution was correct, complete or free of compromise.

Evidence must never silently substitute observations from one implementation revision for claims about another.

---

## 12. Enterprise Acceptance Boundaries

VRP shall not be represented as qualified for a specific critical deployment solely because public validation scenarios pass.

Before enterprise acceptance, an evaluator should establish:

### Security

- Authorized operations succeed.
- Unauthorized operations fail safely.
- Replay and rollback defenses satisfy the agreed contract.
- Canonical state remains protected.

### Continuity

- Logical-session identity is independently observed.
- Supported transport migrations behave as specified.
- Recovery outcomes are accurately classified.

### Operational Behavior

- Resource limits are established.
- Expected interruption is measured.
- Application-level effects are evaluated.
- Failure and recovery conditions are documented.

### Integration

- Compatibility assumptions are explicit.
- Deployment prerequisites are defined.
- Rollback and emergency-disable procedures exist.
- Responsibilities between VRP and dependent systems are established.

### Evidence

- The evaluated release is identifiable.
- Results are reproducible or independently reviewable.
- Limitations are disclosed.
- Acceptance is scoped to the agreed environment.

A pilot acceptance decision shall be based on explicit criteria, not marketing claims.

---

## 13. Prohibited Assurance Shortcuts

The following statements are not justified by this document alone:

- VRP cannot be broken.
- VRP guarantees zero downtime.
- VRP defeats every DDoS or MITM attack.
- VRP guarantees recovery under every blackout.
- VRP has universal formal security proof.
- VRP is independently certified for all critical infrastructure.
- VRP has been approved or deployed by a named organization without supporting evidence.
- A reference-model test establishes production behavior.
- Recovery of connectivity proves preservation of logical-session identity.

Strong engineering requires narrowly stated, falsifiable claims.

A claim is credible only when its evidence supports its exact scope.

---

## 14. Protected Implementation Boundary

This document intentionally excludes:

- Protected Core source code.
- Proprietary internal algorithms.
- Private authorization implementation details.
- Internal recovery mechanisms.
- Secret material.
- Private infrastructure credentials.
- Non-public attack surfaces.
- Restricted deployment configuration.
- Proprietary runtime control logic.

The public assurance interface is based on:

**DECLARED CONTRACTS**

**OBSERVABLE BEHAVIOR**

**REPRODUCIBLE TESTS**

**ATTRIBUTABLE EVIDENCE**

**EXPLICIT LIMITATIONS**

Independent evaluation does not require unrestricted disclosure of protected intellectual property.

Where additional verification requires privileged access, the evaluation scope and disclosure terms must be defined separately.

---

## 15. Review Acceptance Conditions

An assurance claim is eligible for acceptance only when:

1. The claim is precisely defined.
2. The evaluated artifact is identifiable.
3. The test environment is recorded.
4. The adversarial input is attributable.
5. Expected and observed behavior are distinguishable.
6. Negative controls are evaluated.
7. Positive controls are evaluated where applicable.
8. Evidence integrity can be checked.
9. Relevant limitations are disclosed.
10. The published conclusion does not exceed the evidence.

A failure to satisfy these conditions must result in a qualified or inconclusive verdict.

No unverified assumption may be silently promoted into an established security guarantee.

---

## 16. Public Review References

### Official VRP Architecture

https://github.com/Endless33/Veil-Routing-Protocol

### VRP External Validation Kit

https://github.com/Endless33/vrp-validation-kit

Reviewers should pin the exact repository revision used for evaluation rather than relying on a mutable branch name.

---

## 17. Engineering Position

VRP is based on a deliberate architectural separation:

**SESSION != TRANSPORT**

The transport is replaceable.

Logical-session identity is not defined solely by that transport.

Authority must remain valid.

Canonical state must remain protected.

Rejected execution must not silently become authorized execution.

Recovery must preserve correctness rather than merely restore connectivity.

These are security and continuity obligations that can be expressed as contracts and evaluated through adversarial testing.

The ultimate measure of the architecture is not the strength of its claims.

It is whether those claims withstand reproducible attempts to falsify them.

**CONTINUITY FIRST.**

**CORRECTNESS BEFORE CONNECTIVITY.**

**EVIDENCE BEFORE CLAIMS.**

---

**End of Document**

© 2026 Vitalijus Riabovas — Veil Routing Protocol (VRP).

This document describes public assurance requirements and does not grant access to or disclose the protected VRP runtime implementation. Applicable repository licensing and intellectual-property terms remain in force.
