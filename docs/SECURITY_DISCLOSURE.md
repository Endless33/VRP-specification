# Security Disclosure Policy

**Document Version:** Public v1

**Status:** Active

---

# Purpose

This document describes how security-related issues should be reported for the public VRP repository.

Responsible disclosure improves engineering quality while protecting users and ongoing development.

---

# Scope

This policy applies only to the publicly available repository.

It does not apply to protected runtime components, private research, commercial deployments, or confidential engineering assets.

---

# Responsible Disclosure

If you discover a reproducible security issue, please provide sufficient technical information for independent verification.

Useful reports include:

- repository commit
- operating system
- Go version
- reproduction steps
- expected behavior
- observed behavior
- terminal output
- supporting logs
- proof of concept if appropriate

Reports that cannot be reproduced may require additional investigation before they can be evaluated.

---

# Preferred Reports

The most valuable reports include reproducible engineering evidence rather than theoretical discussion.

Examples include:

- race conditions
- deterministic state violations
- replay acceptance
- authority validation failures
- session isolation failures
- memory safety issues
- concurrency defects
- incorrect benchmark behavior
- invariant violations

---

# Out of Scope

The following are generally outside the scope of this public repository:

- speculative vulnerabilities
- unsupported assumptions
- issues requiring unpublished components
- attacks against infrastructure not included in this repository
- social engineering
- denial-of-service against external systems

---

# Coordinated Disclosure

If a report affects public engineering correctness, sufficient time should be allowed for investigation before public discussion.

The objective is accurate engineering, not publicity.

---

# Engineering Philosophy

Security is treated as an engineering process rather than a single feature.

Every meaningful report contributes to improving:

- correctness
- determinism
- verification
- robustness
- reproducibility

---

# No Security Through Obscurity

The public repository is intentionally designed to allow independent review.

Engineering confidence should come from:

- architecture
- implementation
- testing
- verification
- reproducible evidence

rather than secrecy.

---

# Appreciation

Constructive engineering review is appreciated.

Independent verification strengthens the project.

Evidence-based discussion is always preferred over speculation.

Thank you for helping improve the engineering quality of VRP.