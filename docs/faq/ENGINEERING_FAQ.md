# Engineering FAQ

**Document Version:** Public v1

**Status:** Public Engineering

---

# Purpose

This document answers common engineering questions raised during technical evaluation of VRP.

The objective is to clarify the public architecture rather than promote adoption.

---

# Is VRP a VPN?

No.

VRP is a continuity-first runtime architecture.

It is not intended to replace traditional VPN products.

---

# Does VRP replace TCP, UDP, QUIC, or TLS?

No.

VRP operates independently of individual transport technologies.

The runtime manages Logical Sessions while transports may evolve.

---

# Is the runtime open source?

No.

The public architecture and engineering methodology are documented.

The production runtime remains protected.

---

# Why is the runtime protected?

The public architecture is intended for independent engineering evaluation.

The runtime implementation contains proprietary engineering and intellectual property.

---

# Can organizations evaluate VRP without runtime source code?

Yes.

The public repositories are designed around observable engineering behavior.

Evaluation focuses on:

- architecture
- documentation
- validation methodology
- engineering evidence
- reproducibility

---

# Can benchmarks be reproduced?

That is the intended engineering model.

Organizations are encouraged to execute their own benchmarks and compare results.

---

# Can security claims be independently reviewed?

Yes.

The public repositories include:

- security documentation
- threat models
- engineering validation
- reproducible methodology

Organizations should perform their own technical assessment.

---

# Does VRP require replacing existing infrastructure?

No.

The recommended Pilot operates alongside existing infrastructure.

Existing production systems remain in place during evaluation.

---

# What happens if the Pilot does not meet expectations?

The Pilot can simply end.

Organizations should continue only if engineering evidence supports doing so.

---

# Is VRP intended for every organization?

No.

Organizations should evaluate whether continuity-first architecture is relevant to their operational requirements.

---

# Why are there so many engineering documents?

Because engineering confidence should come from documentation, validation, reproducibility, and observable evidence rather than marketing.

---

# Can engineers challenge the published architecture?

Absolutely.

Independent review is encouraged.

Constructive criticism supported by reproducible evidence improves engineering.

---

# Final Principle

Questions are expected.

Engineering skepticism is encouraged.

The objective of this repository is not to eliminate questions.

The objective is to provide enough engineering information for organizations to answer those questions through independent technical evaluation.