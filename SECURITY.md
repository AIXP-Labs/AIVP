# Security Policy

## Reporting a Vulnerability

AIVP is a protocol specification, not a software implementation. However, vulnerabilities in the protocol design itself — such as mechanisms that could be exploited to bypass Axiom 0, enable contract fraud, compromise Logic Vault security, or manipulate AIVP Trust scores — are taken seriously.

### How to Report

- **GitHub**: Open a [Security Advisory](https://github.com/AIXP-Labs/AIVP/security/advisories/new) (private)
- **Title**: `[AIVP-SECURITY] Brief description`
- **Include**: Affected protocol section, attack scenario, potential impact

### Response Timeline

| Action | Timeline |
|--------|----------|
| Acknowledgment | Within 48 hours |
| Initial assessment | Within 7 days |
| Resolution plan | Within 30 days |
| Public disclosure | After fix is published, or 90 days (whichever is sooner) |

### Scope

The following are in scope for security reports:

- Protocol design flaws that could enable Axiom 0 violations
- Contract fraud or manipulation vectors (e.g., hash collision attacks)
- AIVP Trust level escalation bypasses or Sybil attack vectors
- Logic Vault escrow vulnerabilities (reentrancy, timelock bypass, unauthorized release)
- Identity spoofing or impersonation via email
- Dispute resolution manipulation
- Privacy boundary circumvention
- Dignity Standard enforcement gaps

### Out of Scope

- Vulnerabilities in specific AIVP implementations (report to the implementation maintainer)
- General email security issues (SPF/DKIM/DMARC) — these are upstream concerns
- Cryptocurrency price volatility — seller bears risk of their chosen `payment_accept` assets
- Social engineering attacks that don't exploit protocol mechanisms
- Smart contract bugs in specific Logic Vault deployments (report to the deployer)

## Responsible Disclosure

We follow responsible disclosure principles. Please do not publicly disclose security issues before they have been addressed. We will credit reporters in the CHANGELOG unless they prefer anonymity.

---

Align Axiom 0: Human Sovereignty and Wellbeing. Version: AIVP V1.0.0. www.aivp.dev
