
# VRP INDEPENDENT SECURITY & PROTOCOL REVIEW GUIDE

## Veil Routing Protocol (VRP)
### External Technical Evaluation, Reproducible Validation and Evidence-Based Assurance

**Document ID:** VRP-ISPRG-008  
**Version:** 1.0  
**Date:** 2026-11-12  
**Status:** Public Independent Review Procedure — Proposed  
**Classification:** PUBLIC — NON-PROPRIETARY  
**Author:** Vitalijus Riabovas  
**Project:** Veil Routing Protocol / Jumping VPN

---

# 1. Executive Summary

The Veil Routing Protocol (VRP) is an experimental continuity-first networking architecture based on a fundamental distinction:

**SESSION != TRANSPORT**

The logical session is treated as a security-relevant identity that must not be confused with the lifetime of an individual transport connection.

Transport replacement, endpoint migration and network recovery introduce questions that cannot be answered by connectivity testing alone.

A technically meaningful evaluation must determine:

- Whether logical-session identity is preserved.
- Whether authority remains valid.
- Whether invalid replay attempts are rejected.
- Whether canonical state remains protected.
- Whether rejected operations create unauthorized side effects.
- Whether recovery preserves the required security contract.
- Whether legitimate application execution can resume.
- Whether the supporting evidence is reproducible and attributable.

This document defines a structured independent review procedure for external engineers, security researchers, infrastructure architects and enterprise evaluators.

Its purpose is to make VRP's public security claims technically assessable without requiring unrestricted disclosure of proprietary implementation details.

The central review principle is:

**VERIFY OBSERVABLE BEHAVIOR. ATTRIBUTE THE EVIDENCE. STATE THE LIMITATIONS.**

This document does not represent third-party certification, an independent audit already completed, or an unrestricted authorization to test systems outside the reviewer's control.

---

# 2. Review Objectives

An independent VRP review should answer five primary questions.

## 2.1 Architectural coherence

Are the declared protocol invariants internally consistent and sufficiently precise to support implementation and testing?

## 2.2 Security boundary correctness

Does the evaluated system enforce the declared separation between transport reachability, session identity and execution authority?

## 2.3 Adversarial behavior

Does the evaluated artifact preserve the applicable security contract under malformed input, replay, concurrency, endpoint changes and recovery?

## 2.4 Evidence credibility

Can the reviewer attribute each result to a specific artifact, execution environment and test procedure?

## 2.5 Qualification limitations

What conclusions are supported, and what remains unverified?

The reviewer should report these dimensions separately.

A favorable architectural assessment does not automatically establish runtime security.

A passing reference model does not automatically establish integrated-system correctness.

---

# 3. Review Independence

Independent review requires clear attribution of the evaluator's role.

The following categories should be distinguished.

| Review class | Description |
|---|---|
| AUTHOR REVIEW | Evaluation performed by the project author |
| INTERNAL REVIEW | Evaluation performed within the development organization |
| EXTERNAL REPRODUCTION | Public tests reproduced by an external evaluator |
| INDEPENDENT BLACK-BOX REVIEW | External evaluation of observable behavior |
| AUTHORIZED PRIVATE REVIEW | Evaluation under separately agreed access conditions |
| FORMAL CERTIFICATION | Assessment under an identified certification framework |

These categories are not interchangeable.

A public GitHub repository does not automatically imply independent validation.

A successful external reproduction should identify the evaluator, the tested revision and the scope of the result.

The term "independently verified" must not be used unless the corresponding independent evaluation actually occurred.

---

# 4. Public Review Scope

The baseline public review may examine:

1. Published architectural contracts.
2. Security invariant definitions.
3. Threat assumptions.
4. Trust boundaries.
5. Public reference models.
6. Public adversarial test scenarios.
7. Evidence verification tooling.
8. Published remediation records.
9. Public release provenance.
10. Documented limitations.

The reviewer may also evaluate additional authorized artifacts supplied under a separate agreement.

## 4.1 Out-of-scope material

Public review does not automatically include access to:

- Protected Core source code.
- Proprietary algorithms.
- Private authority-resolution mechanisms.
- Confidential implementation details.
- Secret cryptographic material.
- Private deployment infrastructure.
- Non-public security findings.
- Confidential customer or pilot data.

A reviewer's lack of access to these materials must be reflected in the scope of the review.

It must not be concealed or treated as equivalent to a full source-code audit.

---

# 5. Reviewer Prerequisites

The reviewer should have sufficient familiarity with:

- Network protocol engineering.
- Distributed systems.
- Concurrent execution.
- State-machine correctness.
- Authentication and authorization boundaries.
- Replay protection.
- Failure recovery.
- Linux networking.
- Reproducible software testing.
- Security evidence handling.

Specialized knowledge may be required for deployment-specific evaluation.

Examples include telecom mobility, satellite networking or critical infrastructure.

---

# 6. Authorized Testing Boundary

All adversarial testing must be conducted against infrastructure the reviewer owns or is explicitly authorized to evaluate.

The reviewer must not interpret publication of this guide as permission to:

- Attack production networks.
- Scan unrelated infrastructure.
- Disrupt third-party services.
- Target systems without authorization.
- Access private repositories without permission.
- Circumvent contractual access restrictions.

The recommended baseline is an isolated laboratory environment.

Tests involving traffic flooding or resource pressure should have explicit safety limits and a documented termination procedure.

---

# 7. Public Review Materials

The following public project locations provide the starting point for review.

## 7.1 VRP project

https://github.com/Endless33/Veil-Routing-Protocol

## 7.2 VRP validation kit

https://github.com/Endless33/vrp-validation-kit

## 7.3 Related assurance documents

    docs/review/VRP_SECURITY_ASSURANCE_CASE.md

    docs/review/VRP_EVIDENCE_TRACEABILITY_MATRIX.md

    docs/security/VRP_ADVERSARIAL_SECURITY_VALIDATION_REPORT.md

    docs/security/VRP_THREAT_MODEL_AND_TRUST_BOUNDARIES.md

    docs/security/VRP_SECURITY_FINDINGS_AND_REMEDIATION_REGISTER.md

    docs/security/VRP_AUTONOMOUS_DEFENSE_AND_FAIL_CLOSED_DECISION_CONTRACT.md

    docs/validation/VRP_HOSTILE_NETWORK_CONTINUITY_QUALIFICATION_MATRIX.md

These paths identify the proposed documentation set.

The reviewer must verify that the files exist in the selected repository revision.

A document referenced in a review guide must not be treated as published evidence until its actual contents and revision have been inspected.

---

# 8. Review Phase Model

The recommended evaluation sequence is:

    REVIEW SCOPE
         |
         v
    ARTIFACT INVENTORY
         |
         v
    SOURCE IDENTITY
         |
         v
    SPECIFICATION REVIEW
         |
         v
    THREAT MODEL REVIEW
         |
         v
    TEST DISCOVERY
         |
         v
    BASELINE EXECUTION
         |
         v
    ADVERSARIAL REPRODUCTION
         |
         v
    POSTCONDITION VERIFICATION
         |
         v
    EVIDENCE INTEGRITY
         |
         v
    LIMITATION ANALYSIS
         |
         v
    INDEPENDENT VERDICT

The reviewer should preserve the relationship between every claim and its supporting evidence.

---

# 9. Phase 1 — Artifact Inventory

## Objective

Identify the actual artifacts available for evaluation.

## Required actions

The reviewer should record:

- Repository name.
- Repository URL.
- Default or selected branch.
- Commit SHA.
- Source tree SHA.
- Relevant release tag.
- Published documentation.
- Executable test entry points.
- Evidence bundles.
- Verification tools.

## Example repository inspection

    git clone https://github.com/Endless33/vrp-validation-kit.git

    cd vrp-validation-kit

    git rev-parse HEAD

    git rev-parse 'HEAD^{tree}'

    git status --short

    git log -1 --format=fuller

## Required result

The reviewer must identify the exact source revision.

A branch name alone is insufficient as an immutable artifact identity.

---

# 10. Phase 2 — Specification Review

## Objective

Determine whether the published security requirements are sufficiently precise to support testing.

## Review questions

1. Is logical-session identity distinguished from transport identity?
2. Is execution authority distinguished from transport reachability?
3. Are invalid replay operations explicitly prohibited?
4. Are duplicate canonical commits prohibited?
5. Are stale authority and invalid epoch rollback addressed?
6. Are recovery postconditions defined?
7. Are rejection side effects constrained?
8. Are test limitations documented?

## Expected output

A specification review should identify:

- Clear requirements.
- Ambiguous requirements.
- Missing requirements.
- Conflicting requirements.
- Untestable requirements.

The reviewer should not assume that a well-written requirement has already been implemented correctly.

---

# 11. Phase 3 — Threat Model Review

## Objective

Evaluate whether the threat model identifies relevant attacker capabilities and protected assets.

## Review categories

- Network injection.
- Replay.
- Packet reordering.
- Endpoint manipulation.
- Authority abuse.
- Concurrent execution.
- Recovery manipulation.
- Resource exhaustion.
- Evidence tampering.

## Required distinction

The reviewer must distinguish:

**Threat identified**

The threat is included in the model.

**Threat tested**

An attributable test was executed.

**Threat mitigated**

The relevant security postconditions were verified within a declared scope.

**Threat independently verified**

An independent evaluator reproduced the relevant result.

These states must not be collapsed into a single "secure" designation.

---

# 12. Phase 4 — Public Test Discovery

## Objective

Identify executable public tests and their expected behavior.

The reviewer should inspect the selected repository revision rather than assume that documentation commands remain valid.

## Suggested inspection

    find . -maxdepth 4 -type f | sort

    find . -name '*_test.go' -type f | sort

    find ./cmd -maxdepth 3 -type f 2>/dev/null | sort

    go version

## Required record

    TEST_ID

    SOURCE_REVISION

    EXECUTABLE_PATH

    EXPECTED_BEHAVIOR

    REQUIRED_PRECONDITIONS

    REQUIRED_POSTCONDITIONS

    KNOWN_LIMITATIONS

The absence of a test must be reported.

A test name is not proof that the test exercises the claimed security boundary.

---

# 13. Phase 5 — Baseline Execution

## Objective

Establish that the selected public artifact builds and executes in the reviewer's environment.

## Suggested baseline

    go test ./...

    go vet ./...

These commands are applicable when the selected repository revision contains an appropriate Go module and supports these operations.

The reviewer should inspect the repository's documented prerequisites before execution.

## Required observations

- Build result.
- Test result.
- Runtime version.
- Execution environment.
- Exit status.
- Unexpected failures.

## Interpretation

A passing baseline establishes that the evaluated test suite passed in the reviewer's environment.

It does not establish that all protocol invariants are covered.

---

# 14. Phase 6 — Public Replay Validation

## Objective

Reproduce the public replay-containment reference scenario.

## Scenario reference

    VRP-PUB-REPLAY-9999

## Intended behavior

The public reference model evaluates rejection of 9,999 duplicate sequence attempts.

Expected conditions:

    DUPLICATE_ATTEMPTS = 9999

    DUPLICATE_ACCEPTED = 0

    DUPLICATE_REJECTED = 9999

    CANONICAL_STATE = PRESERVED

The next valid operation should remain admissible according to the reference-model contract.

## Suggested execution

    go run ./cmd/vrp-replay-storm-9999

The reviewer must verify that this executable exists in the selected revision.

## Required evidence

- Source revision.
- Execution command.
- Exit status.
- Observed acceptance count.
- Observed rejection count.
- Canonical-state result.
- Positive-control result.

## Interpretation

A successful execution establishes the behavior of the evaluated reference model.

It does not establish production network packet-path enforcement.

The reviewer must not describe a reference-model PASS as a full runtime security certification.

---

# 15. Phase 7 — Adversarial Scenario Evaluation

Adversarial evaluation should be organized around explicit security invariants.

## 15.1 Malformed input

Determine whether invalid input causes unauthorized protected-state mutation.

## 15.2 Replay

Determine whether invalid duplicate operations are rejected.

## 15.3 Concurrent admission

Determine whether concurrent submissions violate the applicable single-admission contract.

## 15.4 Endpoint manipulation

Determine whether rejected traffic can authorize a persistent endpoint change.

## 15.5 Stale authority

Determine whether obsolete authority can perform protected operations.

## 15.6 Epoch rollback

Determine whether invalid historical authority can regain execution rights.

## 15.7 Recovery

Determine whether the security contract remains valid after transport disruption.

## 15.8 Resource pressure

Determine whether the implementation remains within documented resource bounds.

Each scenario requires its own evidence record.

A passing replay test must not be treated as evidence that endpoint manipulation is also secure.

---

# 16. Independent Session Identity Verification

The foundational VRP claim is:

**SESSION != TRANSPORT**

This claim requires special treatment.

A reviewer must not infer logical-session continuity solely from:

- Process survival.
- Socket survival.
- UDP reconnection.
- Interface persistence.
- Tunnel restoration.
- Endpoint replacement.
- Successful packet delivery.

These observations may support a continuity assessment.

They are not sufficient identity evidence.

## 16.1 Required independent observations

    LOGICAL_SESSION_IDENTITY

    TRANSPORT_IDENTITY

    AUTHORITY_STATE

    CANONICAL_STATE

    APPLICATION_PROGRESS

    OBSERVATION_TIMESTAMP

## 16.2 Baseline

Before disruption:

    SESSION_IDENTITY = S0

    TRANSPORT_IDENTITY = T0

## 16.3 Post-disruption

After supported transport recovery:

    SESSION_IDENTITY = S0

The transport identity may change.

The logical-session identity must remain the same for a successful continuity claim.

## 16.4 Required independence

The identity oracle must not simply report that the transport reconnected.

The reviewer must establish that the identity observation is sufficiently independent of the recovery event being tested.

## 16.5 Verdict

If the original logical-session identity cannot be independently established:

    CONTINUITY_VERDICT = INCONCLUSIVE

A successful application packet must not override this classification.

---

# 17. Recovery and Network Impairment Review

A reviewer may evaluate the proposed hostile-network qualification profiles.

## Suggested scenarios

| Scenario | Configuration |
|---|---|
| Endpoint churn | Every 2 seconds |
| Packet loss | 30% |
| Packet loss | 50% |
| Packet loss | 80% |
| Packet loss | 90% |
| Blackout | 10 seconds |
| Blackout | 30 seconds |
| Blackout | 60 seconds |
| High latency | RTT 300–900 ms |
| Reordering | Controlled |
| Replay pressure | During migration |
| Authority conflict | During recovery |
| Compound failure | Churn + 80% loss + blackout + replay |

These are proposed test configurations.

They are not statements of completed qualification.

## Required observations

- Actual impairment parameters.
- Session identity.
- Authority state.
- Canonical-state integrity.
- Replay decisions.
- Application delivery.
- Recovery time.
- Resource consumption.

The reviewer must distinguish configured impairment from measured impairment.

---

# 18. Authority Integrity Review

Authority integrity must be evaluated independently of connectivity.

## Required questions

1. Can obsolete authority modify protected state?
2. Can invalid historical authority regain execution rights?
3. Can rejected traffic authorize a new endpoint?
4. Can recovery bypass authority validation?
5. Can conflicting authority produce unauthorized canonical changes?

## Required postconditions

    UNAUTHORIZED_AUTHORITY_ACCEPTED = FALSE

    INVALID_EPOCH_ROLLBACK = FALSE

    UNAUTHORIZED_CANONICAL_COMMIT = FALSE

The reviewer should verify the actual protected-state outcome.

A rejection message alone is insufficient if unauthorized state mutation occurred before rejection.

---

# 19. Concurrency and Linearizability Review

Concurrency testing should evaluate observable atomicity contracts.

## Required scenarios

- Simultaneous duplicate admission.
- Repeated concurrent execution.
- Concurrent recovery-related operations.
- State observation during mutation.
- Legitimate operation after rejected duplicates.

## Required observations

    CONCURRENT_ATTEMPTS

    ACCEPTED_OPERATIONS

    REJECTED_OPERATIONS

    DUPLICATE_COMMITS

    CANONICAL_STATE

## Important limitation

Race-detector success does not establish linearizability.

A reviewer must verify that concurrent execution preserves the required admission and canonical-state contracts.

---

# 20. RED-to-GREEN Remediation Review

The reviewer should inspect the complete remediation chain for each published security finding.

## Required sequence

    RED
      |
      v
    FAILURE ATTRIBUTION
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
      |
      v
    CLOSURE DECISION

## Required evidence

    FINDING_ID

    AFFECTED_INVARIANT

    RED_SOURCE_REVISION

    RED_EVIDENCE

    CORRECTION_REVISION

    GREEN_EVIDENCE

    REGRESSION_EVIDENCE

    POSITIVE_CONTROL

    RESIDUAL_RISK

## Review rule

A correction commit is not sufficient evidence of verified closure.

The original failure must be attributable.

The corrected behavior must be verified.

The relevant regression contract must remain satisfied.

---

# 21. Evidence Provenance Review

Evidence must be attributable to the evaluated artifact.

## 21.1 Required fields

    SOURCE_REPOSITORY

    SOURCE_COMMIT

    SOURCE_TREE

    BUILD_ID

    TEST_ID

    TEST_CONFIGURATION

    EXECUTION_TIMESTAMP

    EXECUTION_COMMAND

    EXIT_STATUS

    OBSERVED_RESULT

    VERDICT

## 21.2 Integrity verification

Evidence bundles should contain integrity references such as SHA-256 checksums.

The reviewer should verify the checksums against the supplied files.

## 21.3 Authenticity limitation

A checksum establishes consistency with a reference digest.

It does not independently establish the identity of the evidence producer.

Cryptographic signatures, where used, require explicit trust assumptions.

## 21.4 Incomplete evidence

Missing logs, ambiguous source revisions or unavailable postconditions must be reported.

The reviewer must not infer successful behavior from an incomplete evidence bundle.

---

# 22. Evidence Hierarchy

The review should distinguish the following evidence levels.

| Level | Evidence class |
|---|---|
| E0 | Specification contract |
| E1 | Public reference model |
| E2 | Controlled runtime evaluation |
| E3 | Integrated system evaluation |
| E4 | Independent reproduction |
| E5 | Operational qualification |

A higher evidence level does not automatically guarantee correctness.

Every result remains bounded by its actual test scope.

An E1 PASS must not be represented as an E3 PASS.

An E3 PASS must not be represented as E4 independent reproduction without external verification.

---

# 23. Independent Review Finding Classification

Review findings should use a consistent classification.

## CRITICAL

A demonstrated violation with severe security consequences under the documented threat assumptions.

## HIGH

A demonstrated violation materially weakening an important security boundary.

## MEDIUM

A demonstrated bounded security weakness requiring correction.

## LOW

A demonstrated issue with limited security impact.

## INFORMATIONAL

An observation, improvement opportunity or assurance limitation without a demonstrated security violation.

## INCONCLUSIVE

Insufficient evidence to determine whether the relevant security property holds.

Severity must be supported by demonstrated impact and attack preconditions.

A theoretical threat is not automatically a confirmed vulnerability.

---

# 24. Review Finding Template

Every substantive review finding should include:

    REVIEW_FINDING_ID:

    TITLE:

    REVIEWER:

    REVIEW_DATE:

    EVALUATED_REPOSITORY:

    EVALUATED_REVISION:

    AFFECTED_INVARIANT:

    THREAT_CATEGORY:

    PRECONDITIONS:

    REPRODUCTION_STEPS:

    EXPECTED_BEHAVIOR:

    OBSERVED_BEHAVIOR:

    SECURITY_IMPACT:

    EVIDENCE_REFERENCES:

    SEVERITY:

    LIMITATIONS:

    RECOMMENDATION:

    DISCLOSURE_STATUS:

The reviewer should avoid including credentials, secret material or unnecessary proprietary implementation details.

---

# 25. Review Verdict Model

A review should not be reduced to a single unqualified PASS or FAIL.

The recommended report contains separate dimensions.

| Dimension | Permitted verdict |
|---|---|
| Specification clarity | PASS / FAIL / INCONCLUSIVE |
| Threat model completeness | PASS / FAIL / INCONCLUSIVE |
| Public test reproducibility | PASS / FAIL / INCONCLUSIVE |
| Replay containment | PASS / FAIL / INCONCLUSIVE / NOT RUN |
| Authority integrity | PASS / FAIL / INCONCLUSIVE / NOT RUN |
| Canonical-state integrity | PASS / FAIL / INCONCLUSIVE / NOT RUN |
| Session continuity | PASS / FAIL / INCONCLUSIVE / NOT RUN |
| Recovery behavior | PASS / FAIL / INCONCLUSIVE / NOT RUN |
| Resource behavior | PASS / FAIL / INCONCLUSIVE / NOT RUN |
| Evidence provenance | PASS / FAIL / INCONCLUSIVE |

A reviewer may report that a reference model passes while integrated runtime behavior remains unverified.

Such a result is technically valid when its scope is explicit.

---

# 26. Review Acceptance Gates

A formal PASS requires all applicable gates.

## GATE 1 — Artifact Identity

The evaluated source or build is identified.

## GATE 2 — Test Identity

The relevant test is identified.

## GATE 3 — Valid Preconditions

The required initial state is established.

## GATE 4 — Reproducible Execution

The documented test procedure is executed.

## GATE 5 — Expected Behavior

The applicable expected behavior is defined.

## GATE 6 — Observed Postconditions

The relevant security postconditions are measured.

## GATE 7 — Positive Control

Legitimate behavior is verified where required.

## GATE 8 — Evidence Integrity

The evidence record is attributable and internally consistent.

## GATE 9 — Scope Declaration

Limitations and untested conditions are disclosed.

A missing mandatory gate must prevent an unqualified PASS.

---

# 27. Reporting Unverified Claims

A reviewer should use precise language.

## Acceptable

"The public reference model rejected the duplicate inputs under the evaluated conditions."

"The runtime's authority-integrity behavior was not independently evaluated."

"The session reconnected, but logical-session continuity was not independently established."

"The supplied evidence was insufficient to reproduce the reported result."

## Not acceptable without additional evidence

"VRP is immune to replay attacks."

"VRP is fully secure."

"VRP has been independently certified."

"VRP guarantees continuity under all network failures."

"VRP provides exactly-once execution for all applications."

A review must not convert bounded experimental observations into universal guarantees.

---

# 28. Security Disclosure Procedure

Security findings should be handled responsibly.

## 28.1 Initial report

The reviewer should provide:

- A concise description of the issue.
- Affected artifact and revision.
- Reproduction conditions.
- Security impact.
- Supporting evidence.
- Contact information for follow-up.

## 28.2 Private reporting

Findings involving exploitable weaknesses should initially be reported through an explicitly published private security-reporting channel.

The reviewer should inspect the repository's current security policy or maintainer contact information.

This guide does not invent a private security contact address.

## 28.3 Coordinated disclosure

The reporter and maintainer should coordinate a reasonable disclosure timeline according to the issue's severity and the affected users.

## 28.4 Public disclosure

Public reporting should avoid releasing sensitive implementation details or exploit material that unnecessarily increases risk.

## 28.5 Retesting

A corrected finding should be independently retested where possible.

The original RED evidence and final GREEN evidence should remain attributable.

---

# 29. Reviewer Conduct and Technical Integrity

Independent review should be:

- Reproducible.
- Evidence-based.
- Scoped.
- Technically precise.
- Respectful of intellectual property.
- Clear about uncertainty.
- Free from unsupported certification claims.

A reviewer should distinguish between:

**Design disagreement**

A difference of architectural preference.

**Specification ambiguity**

A requirement that cannot be interpreted consistently.

**Implementation defect**

A demonstrated deviation from the applicable contract.

**Security vulnerability**

A demonstrated weakness with security impact under stated attack assumptions.

**Evidence limitation**

A claim that cannot be verified from the available artifacts.

These categories should not be conflated.

---

# 30. Independent Review Report Structure

A completed external review should use the following structure.

    1. REVIEW IDENTIFICATION

    2. REVIEWER AND ORGANIZATION

    3. SCOPE

    4. ARTIFACT INVENTORY

    5. SOURCE REVISIONS

    6. ENVIRONMENT

    7. SPECIFICATION ASSESSMENT

    8. THREAT MODEL ASSESSMENT

    9. EXECUTED TESTS

    10. OBSERVED RESULTS

    11. SECURITY FINDINGS

    12. EVIDENCE VERIFICATION

    13. LIMITATIONS

    14. RECOMMENDATIONS

    15. FINAL SCOPED VERDICT

    16. REVIEW DATE

    17. EVIDENCE REFERENCES

The final report should identify which results were independently reproduced and which were obtained from project-supplied evidence.

---

# 31. Independent Review Completion Checklist

A review is ready for publication when the applicable items are complete.

- [ ] Review scope defined.
- [ ] Evaluated repositories identified.
- [ ] Exact source revisions recorded.
- [ ] Test environment documented.
- [ ] Relevant invariants identified.
- [ ] Threat assumptions documented.
- [ ] Public tests inspected.
- [ ] Baseline tests executed.
- [ ] Adversarial tests executed where applicable.
- [ ] Negative controls evaluated.
- [ ] Positive controls evaluated.
- [ ] Canonical-state postconditions inspected.
- [ ] Authority integrity assessed.
- [ ] Session identity independently verified where claimed.
- [ ] Evidence integrity checked.
- [ ] Failed and inconclusive tests retained.
- [ ] Limitations disclosed.
- [ ] Final verdict scoped.
- [ ] Protected material excluded from public artifacts.

Unchecked items must not be silently treated as completed.

---

# 32. Enterprise Technical Due Diligence

Enterprise evaluators should be able to answer:

1. What security problem does VRP intend to address?
2. Which invariants define the solution?
3. Which invariants have executable public tests?
4. Which results have been independently reproduced?
5. Which results rely on private runtime evidence?
6. Which failure classes have documented remediation?
7. How is logical-session continuity verified?
8. How is execution authority protected during migration?
9. How is recovery distinguished from session recreation?
10. What operational limits remain unqualified?
11. What additional pilot testing is required?
12. What evidence supports the proposed deployment?

These questions should be answered with traceable artifacts rather than unsupported assurances.

---

# 33. Review Limitations

This guide does not establish:

- Full source-code audit coverage.
- Complete cryptographic verification.
- Universal network attack resistance.
- Absence of undiscovered vulnerabilities.
- Production readiness.
- Telecom certification.
- Safety-critical certification.
- Independent validation already performed.
- Qualification of every VRP runtime revision.

The availability of a review procedure is not evidence that the procedure has been executed.

---

# 34. Protected Core Disclosure Boundary

This guide intentionally exposes:

- Public security requirements.
- Review methodology.
- Observable acceptance criteria.
- Test reproduction procedures.
- Evidence requirements.
- Finding classification.
- Disclosure expectations.
- Assurance limitations.

It intentionally excludes:

- Proprietary continuity algorithms.
- Private authority-resolution mechanisms.
- Internal state-machine implementation.
- Protected runtime source code.
- Secret cryptographic material.
- Confidential packet-processing internals.
- Non-public security controls.
- Private deployment configurations.

**PROTECTED CORE DISCLOSURE: NONE**

External evaluation should be designed around observable behavior and independently checkable evidence.

Restricted implementation access may be negotiated separately where appropriate.

---

# 35. Public References

## VRP Project

https://github.com/Endless33/Veil-Routing-Protocol

## VRP Validation Kit

https://github.com/Endless33/vrp-validation-kit

## Normative Terminology

RFC 2119 — Key words for use in RFCs to Indicate Requirement Levels

https://www.rfc-editor.org/rfc/rfc2119

RFC 8174 — Ambiguity of Uppercase vs Lowercase in RFC 2119 Key Words

https://www.rfc-editor.org/rfc/rfc8174

---

# 36. Final Independent Review Statement

The value of an independent protocol review is not determined by whether the reviewer agrees with the project author.

It is determined by whether the evaluation produces reproducible, attributable and technically meaningful evidence.

For VRP, the foundational distinctions remain:

**SESSION != TRANSPORT**

**RECONNECTION != VERIFIED CONTINUITY**

**REACHABILITY != AUTHORITY**

**REPLAY != NEW EXECUTION**

**REJECTION != PARTIAL COMMIT**

**RECOVERY != AUTHORITY RESET**

**REFERENCE MODEL != INTEGRATED RUNTIME**

**AUTHOR TESTING != INDEPENDENT CERTIFICATION**

**SECURITY CLAIM != VERIFIED EVIDENCE**

The purpose of independent review is to determine which of these properties are supported by the evaluated artifact and which require further qualification.

The required engineering discipline is:

**IDENTIFY THE ARTIFACT.**

**REPRODUCE THE TEST.**

**VERIFY THE POSTCONDITIONS.**

**PRESERVE THE EVIDENCE.**

**REPORT THE LIMITATIONS.**

**DRAW ONLY THE CONCLUSIONS THE EVIDENCE SUPPORTS.**

---

**END OF DOCUMENT**

**Veil Routing Protocol — VRP**

**Author:** Vitalijus Riabovas

© 2026 Vitalijus Riabovas. All rights reserved.

This public document defines an independent technical review procedure. It does not disclose proprietary implementation mechanisms or grant access to Protected Core materials. Applicable repository license terms govern reuse.
