# Contributing Guide — Security First

## Before You Contribute

Read these first:
1. **[SECURITY.md](.github/SECURITY.md)** — Vulnerability reporting
2. **[SECURITY_CHECKLIST.md](SECURITY_CHECKLIST.md)** — Secret prevention
3. This guide

## ✅ Repository Security Checklist

Every pull request will be checked against this list:

```
☐ No .env files committed
☐ No API keys in code
☐ No passwords in config files
☐ No private keys in repository
☐ No secrets in Git history
☐ All exposed secrets rotated (if any found)
☐ Scripts have safety warnings
☐ Documentation is accurate
☐ Pre-commit hooks run locally
☐ Code review by maintainer
```

## 🚀 How to Contribute Safely

### Step 1: Set Up Your Local Environment

```bash
# Clone the repository
git clone https://github.com/bttecs/Incident-Response-breach.git
cd Incident-Response-breach

# Install pre-commit hooks (prevents secret commits)
pip install pre-commit
pre-commit install

# Verify installation
pre-commit run --all-files
```

### Step 2: Create Your Branch

```bash
# Never commit directly to main
git checkout -b feature/your-change-name

# Branch naming convention:
# feature/        → new feature
# fix/            → bug fix
# docs/           → documentation
# security/       → security improvement
# ir-phase-N/     → new incident response phase
```

### Step 3: Make Changes

```bash
# Edit files
nano PHASE_2_INVESTIGATE.md

# Check your changes don't contain secrets
git diff | grep -iE "password|api_key|secret|token|key"
# Should return: (nothing)

# Stage your changes
git add PHASE_2_INVESTIGATE.md
```

### Step 4: Commit (Pre-Commit Runs Automatically)

```bash
# Pre-commit hooks automatically run:
# ✅ Detect secrets
# ✅ Check for private keys
# ✅ Validate YAML/JSON syntax
# ✅ Check file sizes

git commit -m "Add Phase 2 investigation guide"

# If pre-commit finds issues:
# - Fix the issues
# - Stage again: git add .
# - Retry commit: git commit -m "..."
```

### Step 5: Push & Create Pull Request

```bash
# Push to your branch
git push origin feature/your-change-name

# Go to GitHub and create a Pull Request
# https://github.com/bttecs/Incident-Response-breach/pulls

# In the PR description, explain:
# - What changed
# - Why it changed
# - Any testing you did
```

### Step 6: Code Review

- A maintainer will review your PR
- They'll check for:
  - Security issues
  - Accuracy of incident response procedures
  - Code quality
  - Documentation completeness
- Address feedback and commit updates
- When approved, maintainer merges to main

---

## 🔒 What NOT to Contribute

### Secret Prevention

❌ **Do NOT commit:**
- API keys, tokens, or passwords
- Private keys (.pem, .key, .pfx files)
- .env files with real values
- Credentials for any service
- Personal information (SSNs, email addresses from real breaches)
- Proof-of-concept exploits for unpatched vulnerabilities

✅ **DO use:**
- `.env.example` with placeholder values
- `[REDACTED]` for sensitive examples
- `your-value-here` for required fields
- Documentation that explains concepts without exposing real data

### Accuracy

❌ **Do NOT change:**
- Golden rules (rules 1–5 in README.md) without discussion
- Phase sequence (CONTAIN before INVESTIGATE)
- Critical safety warnings

✅ **DO update:**
- Broken links
- Outdated tool names
- Incorrect event log IDs
- Better explanations

---

## 📋 Contribution Checklist

Before submitting a PR, verify:

- [ ] Pre-commit hooks ran without errors
- [ ] No secrets in my changes: `git diff HEAD --stat`
- [ ] I tested in a lab environment
- [ ] Documentation is clear and accurate
- [ ] PowerShell scripts have `-WhatIf` mode
- [ ] Changes follow the 6-phase framework
- [ ] I haven't modified .gitignore (unless adding patterns)
- [ ] I haven't changed .github/ files (unless security-related)
- [ ] My PR has a clear title and description

---

## 🆘 If You Accidentally Committed a Secret

**Do this immediately:**

```bash
# 1. Stop pushing
git reset HEAD

# 2. Remove the secret from your local changes
nano .env  # Delete the secret
git rm .env  # Remove from Git

# 3. Ammend your commit (not yet pushed)
git add .
git commit --amend

# 4. Verify it's not in history
git log --patch | grep -i "api_key" # Should be empty

# 5. Push your cleaned commit
git push origin feature/your-change

# 6. Email security@bttecs.com about the near-miss
```

If the secret was **already pushed to GitHub**:

```bash
# 1. Revoke the secret IMMEDIATELY in the service's dashboard
# 2. Follow the emergency process in SECURITY.md
# 3. Email security@bttecs.com
```

---

## 📚 Resources for Contributors

### Incident Response
- **NIST Cybersecurity Framework:** https://nvlpubs.nist.gov/nistpubs/CSWP/NIST.CSWP.04162018.pdf
- **SANS IR Guide:** https://www.sans.org/reading-room/whitepapers/incident/
- **CIS Incident Response:** https://www.cisecurity.org/

### Tools & Scripts
- **Microsoft Defender for Endpoint:** https://docs.microsoft.com/en-us/microsoft-365/security/defender-endpoint/
- **Wazuh Documentation:** https://documentation.wazuh.com/
- **EZ Tools by Eric Zimmermann:** https://ericzimmerman.github.io/

### Security
- **OWASP Secrets Management:** https://cheatsheetseries.owasp.org/cheatsheets/Secrets_Management_Cheat_Sheet.html
- **git-secrets:** https://github.com/awslabs/git-secrets
- **TruffleHog:** https://github.com/trufflesecurity/trufflehog

---

## Questions?

- **Security question:** Email security@bttecs.com
- **Script question:** Open an Issue with `[SCRIPT]` tag
- **Documentation question:** Open an Issue with `[DOCS]` tag
- **Incident response methodology:** Open an Issue with `[IR]` tag

---

**Thank you for contributing securely!**

Last Updated: May 2026
