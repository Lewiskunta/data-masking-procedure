# Data Masking Procedure
**Document Reference:** QMS-SEC-PRO-001  
**Version:** 1.0  
**Classification:** Internal Engineering Only  

---

## What This Is

A complete, implementation-ready data masking procedure built to satisfy 
ISO 27001:2022 (Annex A 8.11, 8.12), PCI DSS v4.0, and GDPR simultaneously. 
It covers structured and unstructured data, technique selection, key management, 
KPI measurement, audit cadences, and incident response.

It is written as a template - all organisation-specific context has been removed. 
It can be adopted directly or adapted to fit an existing QMS structure.

---

## What It Covers

- Data classification levels and masking requirements per level
- Nine approved masking techniques with permitted use cases and compliance notes
- Mandatory technique selection matrix across six environment types
- Implementation requirements for structured and unstructured data
- Key and token management requirements including HSM and KMS constraints
- Ten KPIs with collection methods and breach response thresholds
- Pre-release validation gates (G1–G7)
- Audit programme with four audit types and finding SLA tiers
- Three-tier incident classification with regulatory notification guidance
- Six appendices: request forms, exception process, execution checklist, 
  validation checklist, incident report template, and glossary

---

## Design Notes

### Structure

The procedure follows a fixed sequence: classify, select technique, implement, 
validate, audit. Each section assumes the previous is complete. Masking failures 
in practice almost always trace back to a skipped stage, not a deficient technique.

---

### Why the technique matrix is mandatory

Discretion over technique selection produces inconsistency across teams and audit 
gaps that are hard to close retroactively. The matrix in Section 4.2 removes that 
discretion. The exception process in Appendix B exists for cases where the approved 
technique cannot be applied — but it requires a documented risk assessment and dual 
approval. That cost is deliberate.

Tokenisation is the default for payment and account data rather than FPE because 
FPE implemented without proper key management infrastructure introduces weaknesses 
that are non-obvious to engineers who aren't working in cryptography daily — 
particularly FF3 without correct tweak management. Tokenisation with a properly 
isolated vault is operationally simpler to get right. FPE is listed as an approved 
alternative where format preservation is a hard requirement and the key management 
infrastructure is already in place.

Substitution is preferred over pseudonymisation for dev and QA because in those 
environments the original value is never needed. Tests need realistic format, not 
a traceable link to a real record. Pseudonymisation carries a mapping table that 
must itself be protected. Substitution eliminates that requirement entirely.

---

### Why the KPI targets are zero or 100%

The KPIs set at zero — masking incident rate, false negative rate, unmasked data 
access in non-production — reflect that there is no acceptable frequency for those 
events. Setting a non-zero target normalises incidents. These are binary controls: 
either unmasked data left the production boundary without authorisation or it didn't.

The KPIs set at 100% follow the same logic. 95% masking coverage means 5% of 
sensitive data assets have no active control. That is not a near-miss — it is an 
uncontrolled exposure.

Pipeline execution time is the only KPI with a flexible target because it is 
environment-dependent. The procedure mandates the SLA be defined and measured 
per environment tier; the environment refresh runbook holds the values.

---

### Why the incident classification has three tiers

The distinction between Masking Failure, Potential Breach, and Confirmed Breach 
exists because the regulatory response obligations differ at each tier. GDPR's 
72-hour notification clock starts when you become aware of a breach likely to 
result in risk to individuals — not when you confirm it. Treating every masking 
failure as a confirmed breach produces over-reporting. Treating every failure as 
low-severity until confirmed produces missed notification windows.

The two-hour containment assessment window in Section 9.1 is tight enough to 
preserve the 72-hour GDPR clock while allowing time for an initial access log 
review before escalation.

---

### Scope boundary

Masking at the point of collection — data never stored in unmasked form — is out 
of scope. This procedure governs data that already exists unmasked and needs to 
cross an environment boundary or be shared externally. Tooling is also out of 
scope; tool selection changes faster than governance procedures should. The 
procedure defines outcome requirements; implementation runbooks hold the 
tool specifics.

---

## Regulatory Alignment

| Framework | Relevant Requirements | Where Addressed |
|---|---|---|
| ISO 27001:2022 | Annex A 8.11 (data masking), 8.12 (data leakage prevention) | Sections 4, 5, 8 |
| PCI DSS v4.0 | Req 3.3 (SAD), 3.4 (PAN), 3.5 (key management) | Sections 3.2, 4.1, 5.4 |
| GDPR | Article 25 (privacy by design), Article 32 (security of processing) | Sections 3, 4.1, 6, 9 |
| NIST SP 800-38G | FF1/FF3-1 for format-preserving encryption | Section 4.1 |
| NIST SP 800-57 | Key management recommendations | Section 5.4 |

---

## How to Use This Template

1. Read Section 3.1 and map your data assets to the classification levels
2. Run automated discovery against your data estate to identify sensitive columns
3. Use the technique selection matrix in Section 4.2 to define your masking rules
4. Implement the validation gates in Section 7.1 before any masked dataset is released
5. Assign the roles in Section 2.4 to named individuals in your organisation
6. Set your environment refresh SLAs in your runbook and reference them from Section 6
7. Replace all placeholder approval fields in the appendices with your organisation's 
   sign-off process

---

## Document

The full procedure document is available on request or via my portfolio.

---

*This template is shared for portfolio and reference purposes. 
It does not constitute legal or compliance advice.*
