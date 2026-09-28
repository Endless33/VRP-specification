Frequently Asked Questions (FAQ)

Document Version: Public v1

Status: Public

---

Purpose

This document answers common questions about the public goals, engineering methodology, and architectural direction of VRP.

It focuses on observable engineering concepts and intentionally avoids protected implementation details.

---

Is VRP a VPN?

No.

VRP explores a continuity-oriented communication architecture.

Although secure communication may be one aspect of a deployment, the project is not intended to be understood as a traditional VPN implementation.

---

Why is the project so heavily tested?

The engineering objective is confidence through reproducible validation.

Observable behaviour should be supported by repeatable testing, engineering evidence, and independent verification whenever practical.

---

Why are there so many engineering documents?

The documentation reflects the engineering methodology of the project.

Separate documents describe architecture, validation, security, binary analysis, and engineering principles to make the validation process easier to understand and independently review.

---

Why perform reverse engineering on your own binaries?

Public binaries should be treated as observable artifacts.

Engineering validation includes binary inspection to better understand the externally visible characteristics of the compiled runtime.

Security should not depend on binaries remaining unexplored.

---

Why is reproducibility important?

Engineering results become significantly more valuable when another engineer can independently obtain equivalent observable results.

Reproducibility strengthens confidence in engineering conclusions.

---

Why collect engineering evidence?

Validation reports, execution logs, binary fingerprints, and other engineering artifacts provide objective support for observable behaviour.

Evidence allows engineering conclusions to be reviewed rather than assumed.

---

Does the public documentation describe the entire implementation?

No.

The public documentation explains architectural objectives, engineering methodology, and observable behaviour.

Protected runtime implementation, proprietary engineering mechanisms, and confidential implementation details are intentionally excluded.

---

Can VRP replace every networking solution?

No.

VRP was developed to explore a specific architectural direction.

Many networking environments are appropriately served by existing technologies.

The suitability of VRP depends on the engineering objectives of a particular deployment.

---

Is the project still evolving?

Yes.

Engineering validation is treated as a continuous process.

Additional testing, documentation, validation methods, and operational improvements may be introduced as the project evolves.

---

Conclusion

The purpose of this FAQ is to clarify the public engineering goals of VRP.

The project emphasizes continuity-oriented architecture, reproducible validation, engineering evidence, and observable behaviour while preserving clear boundaries around protected implementation.