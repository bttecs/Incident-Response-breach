# Security Policy — Vulnerability Disclosure

## 🔒 Reporting Security Vulnerabilities

If you discover a security vulnerability in this incident response framework or scripts, **please report it responsibly**.

### How to Report
1. **DO NOT** open a public GitHub issue
2. **DO NOT** post details in Discussions, Wiki, or anywhere public
3. **DO** email: [security@bttecs.com](mailto:security@bttecs.com) with:
   - **Subject:** `[SECURITY] Vulnerability in Incident-Response-breach`
   - **Description:** What is the vulnerability?
   - **Impact:** What could an attacker do?
   - **Proof of Concept:** How to reproduce (if safe to share)
   - **Suggested Fix:** Your recommendation (if any)

### Response Timeline
- **Acknowledgment:** Within 24 hours
- **Initial Assessment:** Within 3 business days
- **Patch Release:** Within 7 days (for critical issues) or 30 days (for moderate)
- **Public Disclosure:** After patch is released, or 90 days (whichever comes first)

---

## 🚨 Common Vulnerabilities in This Repo

### What We Consider Vulnerabilities
- ✅ Hardcoded secrets (API keys, passwords, tokens) in code
- ✅ Unsafe PowerShell script execution (requires bypass of protections)
- ✅ Evidence collection script that deletes data without confirmation
- ✅ Firewall rules that open dangerous ports unintentionally
- ✅ Log analysis that leaks sensitive data in plain text

### What We Do NOT Consider Vulnerabilities
- ❌ Lack of input validation on `.env.example` (it's a template, not production code)
- ❌ Documentation that explains how to bypass security controls (it's educational)
- ❌ Unsigned commits (use GPG signing if you want strict verification)
- ❌ HTTP instead of HTTPS in examples (clarify it's for local use only)

---

## 🛡️ Security Features Already Implemented

### Repository
- ✅ `.gitignore` prevents accidental secret commits
- ✅ `.env.example` template has no real credentials
- ✅ Branch protection on `main` requires code review
- ✅ All scripts documented with security warnings
- ✅ No dependencies with known vulnerabilities

### Scripts
- ✅ Evidence collection preserves chain of custody
- ✅ Log analysis uses local, offline tools (no data exfiltration)
- ✅ Firewall hardening with default-deny and explicit allows
- ✅ PowerShell scripts require Administrator privileges
- ✅ All scripts have `-WhatIf` preview mode

### Documentation
- ✅ Clear warnings about not powering off machines (destroys evidence)
- ✅ Instructions on preserving evidence legally
- ✅ Breach notification templates compliant with GDPR, HIPAA, PCI-DSS
- ✅ Checklist format prevents skipped critical steps

---

## 🔄 Update & Patch Policy

### How We Release Updates
1. Security fix developed and tested
2. Code review by at least 2 maintainers
3. Security advisory prepared (if public disclosure needed)
4. Patch released with version bump
5. Public disclosure (if applicable)

### Versioning
- **Major.Minor.Patch** (e.g., 1.0.1)
- Patch bumps (1.0.0 → 1.0.1) = security fixes
- Minor bumps (1.0 → 1.1) = new features
- Major bumps (1 → 2) = breaking changes

### Staying Updated
```bash
# Check for updates
git fetch origin
git log --oneline origin/main -5

# Pull latest security patches
git pull origin main

# Subscribe to releases (GitHub only notifies if you "Watch" the repo)
# Click "Watch" → "Custom" → check "Releases"
```

---

## 🚫 What NOT to Do

### Don't
- ❌ Run `git push -f` without team approval (can corrupt shared history)
- ❌ Commit real API keys or passwords (even in private branches — they're still in history)
- ❌ Use `/dev/random` for cryptography (use `/dev/urandom` or `System.Security.Cryptography`)
- ❌ Store evidence on shared cloud drives without encryption
- ❌ Leave PowerShell windows open with administrator access
- ❌ Run scripts downloaded from the internet without reviewing first

### Do
- ✅ Review all scripts before running (especially .ps1 files)
- ✅ Test scripts in a lab environment first
- ✅ Keep external evidence drives physically locked
- ✅ Use `-WhatIf` to preview destructive actions
- ✅ Rotate credentials immediately after a breach
- ✅ Document every action you take (for legal defensibility)

---

## 📋 Security Checklist — Before Using Scripts

Before running **any** script from this repo:

- [ ] I have reviewed the entire script line-by-line
- [ ] I understand what it does (no mysterious code)
- [ ] I understand what data it collects and where it stores it
- [ ] I have tested it in a lab environment first
- [ ] I have an external drive ready (if collection script)
- [ ] I have a backup of critical data (if removal script)
- [ ] I ran with `-WhatIf` mode first (if applicable)
- [ ] I have approval from my manager/CISO
- [ ] I understand the legal implications (evidence preservation)

---

## 🔐 Security Best Practices

### Using This Repository in Production

1. **Incident Response Preparation**
   - [ ] Clone this repo to an internal Git server (if possible)
   - [ ] Review all scripts quarterly
   - [ ] Test scripts in a lab annually
   - [ ] Keep scripts updated with latest patches

2. **During an Incident**
   - [ ] Never run unfamiliar scripts
   - [ ] Always preserve evidence before remediation
   - [ ] Document everything (time, who, what, why)
   - [ ] Keep evidence chain-of-custody intact

3. **After an Incident**
   - [ ] Review lessons learned
   - [ ] Update incident response procedures
   - [ ] Share findings (anonymized) with the team
   - [ ] Report to appropriate parties (legal, auditors)

### For Remote Incident Response

If using this repo for **remote IR** (managing systems remotely):

- ✅ Use encrypted VPN to access systems
- ✅ Use multi-factor authentication on all accounts
- ✅ Log all remote access sessions
- ✅ Store evidence on secure, isolated network drives
- ✅ Rotate passwords after incident

---

## 📞 Support Channels

### Questions About
| Topic | Contact |
|-------|---------|
| **Security vulnerabilities** | security@bttecs.com |
| **Script errors/bugs** | GitHub Issues (public) |
| **Incident response methodology** | INCIDENT_RESPONSE_GUIDE.md |
| **Script usage** | SCRIPTS/README.md |

---

## 📄 License & Legal

This incident response framework is provided as-is for educational and authorized use only.

- **Educational Use:** Learning incident response techniques
- **Authorized Use:** Investigating breaches on systems you own or have permission to investigate
- **Prohibited Use:** Unauthorized access, hacking, or investigation of systems without permission

See LICENSE file for full terms.

---

**Last Updated:** May 2026  
**Policy Version:** 1.0  
**Next Review:** Quarterly or after any reported security issue
