# Frequently Asked Questions (FAQ)

## Status

Public Pilot Documentation

Version: 2.0

---

# Purpose

This document answers common engineering questions regarding the VRP Pilot Program.

The purpose is to clarify the evaluation process, architectural boundaries and expectations before a Pilot begins.

---

# Is VRP open source?

No.

The protected runtime is proprietary.

This repository documents the observable architecture, engineering principles and evaluation methodology.

---

# Can the runtime be evaluated without source code?

Yes.

The Pilot is specifically designed to evaluate observable runtime behavior rather than implementation.

Engineering decisions should be based on reproducible evidence.

---

# Why is the implementation protected?

The objective of the Pilot is to evaluate engineering behavior.

Disclosure of proprietary implementation is not required to determine whether the observable behavior satisfies technical requirements.

---

# What is actually evaluated?

Typical evaluation areas include:

- logical session continuity
- transport evolution
- authority transitions
- deterministic runtime behavior
- replay rejection
- duplicate execution protection
- recovery
- evidence generation

---

# Is VRP a VPN?

No.

VRP is a continuity-first runtime architecture.

It may operate alongside VPN technologies, but it is not defined by tunneling or encryption alone.

---

# Does VRP replace TCP, QUIC or HTTP?

No.

VRP operates at a different architectural level.

It coordinates runtime continuity independently of the transport protocol.

---

# Which transport technologies are supported?

The public architecture is transport-independent.

Support depends on the protected runtime implementation.

Examples may include:

- Ethernet
- Wi-Fi
- LTE
- 5G
- relay infrastructure
- future transport technologies

---

# Can VRP survive transport changes?

Observable continuity across transport evolution is one of the primary engineering objectives evaluated during the Pilot.

Validation scenarios are defined jointly before execution.

---

# Can participants inspect internal algorithms?

No.

Protected implementation remains outside the evaluation scope.

The Pilot evaluates observable behavior only.

---

# Can participants inspect source code?

No.

Source code is not part of the standard Pilot.

---

# What evidence is produced?

Examples include:

- validation reports
- runtime verdicts
- audit summaries
- engineering evidence
- reproducibility reports

Evidence is intended for technical review.

---

# Who owns the generated evidence?

Ownership and permitted use are determined by the applicable Pilot agreement.

Protected implementation remains the property of the runtime owner.

---

# Can Pilot results be reproduced?

Yes.

The evaluation process is designed around reproducibility.

Equivalent observable scenarios should produce equivalent engineering conclusions.

---

# Does Pilot participation guarantee commercial access?

No.

Participation in the Pilot does not guarantee licensing, commercial agreements or future deployment.

Each engagement is evaluated independently.

---

# Can organizations request additional validation scenarios?

Yes.

Additional engineering scenarios may be proposed during evaluation planning.

Their inclusion depends on technical feasibility and evaluation scope.

---

# Can vulnerabilities be reported?

Yes.

Responsible disclosure is encouraged.

Reports should include sufficient engineering detail to reproduce the observed behavior.

---

# Does the Pilot expose cryptographic material?

No.

Protected keys, implementation details and confidential runtime mechanisms remain outside the evaluation boundary.

---

# Why is the architecture documented publicly?

Public documentation enables engineering understanding.

Protected implementation preserves intellectual property.

Both objectives are intentionally separated.

---

# Who should participate?

Organizations that need evidence-based evaluation of communication continuity, deterministic runtime behavior and failure recovery.

The Pilot is intended for engineering teams rather than marketing audiences.

---

# Related Documents

- PILOT_GUIDE.md
- EVALUATION_PROCESS.md
- REQUIREMENTS.md
- SECURITY_BOUNDARIES.md
- NDA.md

---

# Summary

The Pilot exists to answer technical questions through observable engineering behavior.

Implementation remains protected.

Evidence remains reproducible.

Engineering conclusions remain independent.

---

> Questions are encouraged.

> Observable behavior provides the answers.

> Engineering evidence supports the decision.