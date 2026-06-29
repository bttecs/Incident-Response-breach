# Incident Response — Comprehensive Breach Response Roadmap

**Project:** Incident Response Breach Handling & Forensic Investigation  
**Organization:** BTTECS  
**Prepared By:** Security Team  
**Last Updated:** May 2026

---

## Executive Summary

This repository contains a complete incident response framework covering breach detection, containment, forensic investigation, and recovery. It includes ready-to-run PowerShell scripts for evidence collection, log analysis, firewall hardening, and breach documentation.

The framework follows NIST RMF principles and includes templates for breach notification, forensic reports, and remediation planning.

---

## Phase Overview: 6-Stage Response Roadmap

### Phase 1: CONTAIN (Immediate — Hours 0–2)
**Goal:** Stop the bleeding. Isolate the breach, prevent data exfiltration, preserve evidence.

**Actions:**
- [ ] Identify affected systems and networks
- [ ] Isolate compromised machine from network (if necessary without destroying evidence)
- [ ] Disable compromised user accounts (change passwords, revoke MFA, revoke SSH keys)
- [ ] Block attacker IP addresses at firewall
- [ ] Uninstall unauthorized remote access tools
- [ ] Take forensic image/collect volatile data BEFORE any reboots

**Critical:** Do NOT power off the machine immediately — this destroys RAM-resident evidence (running processes, network connections, decryption keys). Only power off if ransomware is actively encrypting files in real time.

**Tools:**
- Invoke-IRCollect.ps1 — Collects volatile data and evidence to external drive
- Remove-SuspectTools.ps1 — Removes attacker-installed backdoors

---

### Phase 2: INVESTIGATE (Hours 2–24)
**Goal:** Understand what happened. Determine scope, timeline, data affected.

**Actions:**
- [ ] Collect event logs from all systems
- [ ] Analyze file access logs — what data was opened?
- [ ] Check SRUM database — how many bytes left the machine?
- [ ] Analyze shell bags and browser history
- [ ] Review jump lists — which files did the attacker open?
- [ ] Check for unauthorized cloud uploads (Google Drive, OneDrive, Dropbox)
- [ ] Geolocate attacker IPs from firewall logs
- [ ] Interview users — when did they first notice something?

**Tools:**
- Chainsaw + Hayabusa — Hunt through EVTX files for IOCs
- EZ Tools (JLECmd, LECmd, PECmd, MFTECmd, SrumECmd)
- Invoke-LogAnalysis.ps1 — Automated multi-tool analysis
- Invoke-ExfilAnalysis.ps1 — Data exfiltration analysis

---

### Phase 3: ERADICATE (Hours 24–72)
**Goal:** Remove the attacker's tools and access.

**Actions:**
- [ ] Uninstall all unauthorized RMM/remote access tools
- [ ] Remove attacker-created user accounts
- [ ] Delete malicious scheduled tasks
- [ ] Remove persistence mechanisms (run keys, startup items, services)
- [ ] Clear firewall rules allowing attacker access
- [ ] Scan with updated antivirus — full system scan
- [ ] Check for kernel-level rootkits (if suspected)
- [ ] Reset admin credentials

**Tools:**
- Remove-SuspectTools.ps1 — Automated removal with logging
- Windows Defender full scan
- Kaspersky Rescue Disk (if rootkit suspected)

---

### Phase 4: RECOVER (Hours 72–168)
**Goal:** Restore normal operations.

**Actions:**
- [ ] Restore systems from clean backups (if pre-breach backups exist)
- [ ] Patch all systems — especially OS and critical applications
- [ ] Update antivirus and malware definitions
- [ ] Re-enable monitoring and logging on all systems
- [ ] Verify backups are working — test restore
- [ ] Rebuild systems that cannot be cleaned
- [ ] Re-enable user access gradually with monitoring

---

### Phase 5: REMEDIATE (Hours 168–720)
**Goal:** Fix the security gaps that allowed the breach.

**Actions:**
- [ ] Enable firewall default-deny inbound policies
- [ ] Enable Script Block Logging on all PowerShell systems
- [ ] Enable file access auditing (EID 4663) on sensitive folders
- [ ] Implement endpoint detection & response (EDR) or improved monitoring
- [ ] Update firewall rules to restrict RDP, SMB, RPC from internet
- [ ] Upgrade end-of-life software (Microsoft Office 2010 → current)
- [ ] Implement MFA on all administrative accounts
- [ ] Deploy centralized log aggregation (Wazuh, Splunk, ELK)
- [ ] Conduct security awareness training

**Configuration:**
- Invoke-TaxFirmFirewallHarden.ps1 — Implements best-practice firewall policies

---

### Phase 6: COMPLY (Hours 720–infinite)
**Goal:** Report, document, and prevent recurrence.

**Actions:**
- [ ] Send breach notification letters to affected parties (if required by law)
- [ ] File breach notification with state attorney general (if applicable)
- [ ] Notify credit bureaus if SSNs were exposed
- [ ] Document incident timeline and findings
- [ ] Prepare for regulatory audits (IRS, state, customer)
- [ ] Conduct post-incident review / lessons learned
- [ ] Update incident response plan based on findings
- [ ] Implement continuous monitoring and quarterly reviews

**Templates:**
- Breach_Notification_Letter.docx
- Incident_Response_Report.md
- Remediation_Plan_Template.md

---

## Critical Golden Rules

### Rule 1: Don't Power Off Immediately
**Why:** RAM contains running processes, active network connections, and sometimes decryption keys. Powering off destroys this evidence permanently.

**When it's OK to power off:** Only if ransomware is actively encrypting files in real-time and you need to stop it immediately.

**What to do instead:** 
- Collect volatile data first (memory dump, running processes, network connections)
- Use Invoke-IRCollect.ps1 to automate this

### Rule 2: Use a Clean Device for Investigation
**Why:** A compromised machine's browser, email client, and DNS resolver may all be tampered with.

**What to do:**
- Perform all analysis on a different machine
- Use the external drive you collected evidence on
- Never plug an infected machine into your analysis network

### Rule 3: Preserve Evidence Before Analysis
**Why:** Log rotation will overwrite events, and attacker cleanup routines may delete logs after the fact.

**What to do:**
- Copy logs to external drive immediately (Invoke-IRCollect.ps1 does this)
- Generate SHA-256 hash manifest (proves integrity)
- Use write-once storage if available

### Rule 4: Sequence Matters
**Critical sequence:**
1. CONTAIN first (isolation, stop exfiltration)
2. INVESTIGATE second (collect evidence, understand scope)
3. ERADICATE third (remove attacker access)
4. RECOVER fourth (restore systems)
5. REMEDIATE fifth (fix security gaps)
6. COMPLY sixth (notify, document, report)

**Why:** Jumping to Phase 5 before Phase 1 is like locking a door while the attacker is still inside.

### Rule 5: Email Is the Master Key
**Why:** Every "forgot my password" link goes to email. Compromising email gives access to all password resets.

**First action:** 
- Reset the email account password
- Enable MFA on email if not already enabled
- Check email forwarding rules for hidden attacker access
- Review email sent items during breach window

---

## Quick Start — Use These Scripts in Order

### Step 1: Collect Evidence (Hour 0)
```powershell
# Right-click PowerShell → Run as Administrator
.\Invoke-IRCollect.ps1 -DriveLetter E -DaysBack 14

# Output: External drive with forensic image, all logs, volatile data
# Time: 20–40 minutes
# Next: Move to clean analysis machine
```

### Step 2: Analyze Logs (Hour 1)
```powershell
# On a CLEAN analysis machine
.\Invoke-LogAnalysis.ps1

# Output: Chainsaw + Hayabusa results, threat timeline
# Time: 5 minutes (tools download on first run)
# Next: Review HTML report and ALERT_* CSV files
```

### Step 3: Check for Data Exfiltration (Hour 2)
```powershell
.\Invoke-ExfilAnalysis.ps1

# Output: Which files were accessed, opened, or transmitted
# Time: 10–15 minutes
# Next: Review LNK_Resolved_Targets.csv and archives
```

### Step 4: Deep Forensics (Hour 3)
```powershell
# Only if exfiltration is confirmed
# (Follow the Tier 1 methods in the investigation guide)
```

### Step 5: Harden Firewall (Hour 24)
```powershell
# After investigation complete
.\Invoke-TaxFirmFirewallHarden.ps1 `
    -LegitimateScreenConnectID "bec82a294218ab1e" `
    -MSPSubnet "203.0.113.50/32"

# Output: Firewall policies enforced, HTML compliance report
# Time: 5 minutes
# Next: Test ProSeries e-file, test MSP access
```

---

## Repository Structure

```
Incident-Response-Breach/
├── README.md (this file)
├── PHASE_1_CONTAIN.md                        # Immediate containment steps
├── PHASE_2_INVESTIGATE.md                    # Forensic investigation guide
├── PHASE_3_ERADICATE.md                      # Removing attacker tools
├── PHASE_4_RECOVER.md                        # Restoration procedures
├── PHASE_5_REMEDIATE.md                      # Security improvements
├── PHASE_6_COMPLY.md                         # Notification & reporting
│
├── SCRIPTS/
│   ├── Invoke-IRCollect.ps1                  # Evidence collection (Windows)
│   ├── Invoke-IRRunbook.ps1                  # Automated IR evidence gather
│   ├── Invoke-LogAnalysis.ps1                # Chainsaw + Hayabusa automation
│   ├── Invoke-ExfilAnalysis.ps1              # Data exfiltration detection
│   ├── Remove-SuspectTools.ps1               # Remove attacker backdoors
│   ├── Invoke-TaxFirmFirewallHarden.ps1      # Firewall hardening for tax firms
│   ├── Deploy-WazuhAgent.ps1                 # Deploy Wazuh security agent
│   └── wazuh-manager-install.sh              # Deploy Wazuh SIEM (Linux)
│
├── TEMPLATES/
│   ├── Breach_Notification_Letter.docx       # GDPR/state law notification
│   ├── Suspect_Tool_Removal_Checklist.docx   # Step-by-step removal
│   ├── Incident_Response_Report.md           # Post-incident report
│   ├── Remediation_Plan_Template.md          # 30/60/90-day fix plan
│   └── Chain_of_Custody_Template.txt         # Evidence integrity tracking
│
├── TOOLS/
│   ├── Log_Analysis_Guide.md                 # Where to find every log file
│   ├── Firewall_Configuration_Guide.md       # Step-by-step firewall setup
│   ├── EZ_Tools_Quick_Reference.md           # JLECmd, LECmd, PECmd usage
│   └── SIEM_Deployment_Guide.md              # Wazuh/Splunk setup
│
├── EVIDENCE/
│   └── IR_Log_Analyzer.html                  # Browser-based log analyzer
│
└── REFERENCE/
    ├── NIST_IR_Framework.md
    ├── CIS_Incident_Response.md
    ├── Attacker_IOC_Templates.md
    └── Legal_Notification_Requirements.md
```

---

## Log Files — Quick Reference

| Log | Location | What It Tells You |
|-----|----------|---|
| **Security** | `C:\Windows\System32\winevt\Logs\Security.evtx` | Logins (4624), failed logins (4625), privilege use (4672), account changes (4720–4732), group membership (4799), object access (4663) |
| **PowerShell** | `C:\Windows\System32\winevt\Logs\Microsoft-Windows-PowerShell%4Operational.evtx` | All PowerShell commands run (if Script Block Logging enabled — Event 4104) |
| **Sysmon** | Event Log → Applications and Services Logs → Sysmon | Process creation (1), network connections (3), file writes (11), registry (12–14), DNS queries (22) |
| **Windows Defender** | Event Viewer → Applications and Services → Windows Defender → Operational | Malware detections (Event 1006–1119) |
| **Task Scheduler** | `Microsoft-Windows-TaskScheduler%4Operational.evtx` | Scheduled tasks created, modified, executed |
| **System** | `C:\Windows\System32\winevt\Logs\System.evtx` | Services started/stopped (7045), system shutdowns, driver loading |
| **Remote Desktop** | `Microsoft-Windows-TerminalServices-RemoteConnectionManager%4Operational.evtx` | RDP connections from network |
| **Firewall** | `C:\Windows\System32\LogFiles\Firewall\pfirewall.log` | Inbound/outbound connections blocked/allowed |
| **DNS** | Router/DNS server logs | Where the attacker's C2 domain is hosted |
| **Network** | Router, managed switch, NetFlow | Unusual data volumes, external IPs, port scanning |

---

## Incident Response Checklist — Print and Keep on Your Desk

```
HOUR 0-2: CONTAIN
───────────────────
☐ Identify affected systems (what was compromised?)
☐ Notify incident response team
☐ Collect volatile data (Invoke-IRCollect.ps1)
☐ Disable attacker accounts (change passwords, disable, revoke MFA)
☐ Block attacker IP addresses at firewall
☐ Uninstall unauthorized RMM tools
☐ Preserve evidence to external drive
☐ Alert C-suite / executive sponsor

HOUR 2-24: INVESTIGATE
─────────────────────────
☐ Analyze collected logs (Invoke-LogAnalysis.ps1)
☐ Check for data exfiltration (Invoke-ExfilAnalysis.ps1)
☐ Review browser history for cloud uploads
☐ Geolocate attacker IPs
☐ Calculate timeline of attack
☐ Determine what data was accessed
☐ Prepare preliminary findings report
☐ Notify legal if breach notification may be required

DAY 1-3: ERADICATE
────────────────────
☐ Remove all unauthorized RMM tools (Remove-SuspectTools.ps1)
☐ Delete attacker-created user accounts
☐ Remove persistence mechanisms
☐ Clear malicious firewall rules
☐ Full antivirus scan of all systems
☐ Patch all critical vulnerabilities
☐ Verify attacker tools are completely removed
☐ Test that legitimate access still works

DAY 3-7: RECOVER
─────────────────
☐ Restore systems from clean backups
☐ Update antivirus definitions
☐ Re-enable security monitoring
☐ Test system functionality
☐ Gradually restore user access
☐ Monitor for signs of reinfection

WEEK 2-4: REMEDIATE
─────────────────────
☐ Harden firewall (Invoke-TaxFirmFirewallHarden.ps1)
☐ Enable Script Block Logging on all PowerShell
☐ Enable file access auditing (EID 4663)
☐ Deploy endpoint detection & response (EDR)
☐ Upgrade end-of-life software
☐ Implement MFA on all admin accounts
☐ Deploy centralized logging (Wazuh, Splunk)
☐ Conduct security training

MONTH 2+: COMPLY
──────────────────
☐ Draft breach notification letters (if required)
☐ File state attorney general reports
☐ Notify credit bureaus
☐ Prepare regulatory audit documentation
☐ Post-incident review meeting
☐ Update incident response plan
☐ Implement quarterly security reviews
```

---

## When to Call a Professional IR Firm

Call Kroll, Mandiant, or a local DFIR consultant if:

- [ ] Confirmed ransomware infection
- [ ] Evidence of lateral movement to multiple systems
- [ ] Confirmed theft of >1,000 customer records
- [ ] Suspected insider threat or nation-state attacker
- [ ] Compromised backup systems (attacker may have destroyed them)
- [ ] Multiple locations compromised
- [ ] Suspect data was transmitted to foreign countries
- [ ] Incident involves PII/PHI/payment card data (HIPAA/PCI breach)
- [ ] Class action lawsuit is likely

**Cost:** $500–5,000/day, but worth it if:
- Evidence will be used in court
- Forensic report needed for regulatory compliance
- Investigating potential prosecutable cybercrime

---

## Regulatory Notification Requirements

| Regulation | Timeline | Who to Notify | Criteria |
|---|---|---|---|
| **GDPR** | 72 hours | Data Protection Authority + Individuals | Any personal data breach |
| **HIPAA** | 60 days | HHS OCR + individuals | Protected health information (PHI) exposed |
| **PCI-DSS** | Variable | Card processor + acquirer | Payment card data exposed |
| **State Laws** (US) | 30–60 days | State AG + individuals | PII or SSN exposed |
| **FTC Safeguards** | 30 days | Customers | Tax firm/financial data breach |
| **IRS** | 30 days | IRS Security Awareness | Breach of tax return data |

---

## Support & Escalation

**Questions about scripts?**  
Check SCRIPTS/ directory — each .ps1 has inline comments and usage examples

**Questions about legal notification?**  
Review TEMPLATES/Breach_Notification_Letter.docx — fill in your state and data type

**Questions about firewall rules?**  
See TOOLS/Firewall_Configuration_Guide.md for step-by-step GUI and PowerShell methods

**Need a professional forensic report?**  
Contact: Kroll, Mandiant, CrowdStrike Services, or local DFIR consultant

---

## Revision History

| Version | Date | Changes | Author |
|---------|------|---------|--------|
| 1.0 | May 2026 | Initial incident response framework | BTTECS |

---

**Last Updated:** May 2026  
**Next Review:** Quarterly after incidents, annually minimum  
**Prepared By:** BTTECS Security Team  
**Classification:** Internal Use / Client Confidential
