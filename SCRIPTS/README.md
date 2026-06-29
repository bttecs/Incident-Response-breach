# Incident Response Tools & Scripts

This directory contains ready-to-run PowerShell scripts for Windows incident response.

## Scripts Overview

### Invoke-IRCollect.ps1
**Purpose:** Automated evidence collection from a potentially compromised Windows system.

**What it does:**
- Captures volatile data (running processes, network connections, users)
- Exports all event logs as .evtx files
- Collects registry hives
- Scans for suspicious files and artifacts
- Creates chain-of-custody SHA256 manifest
- Generates HTML summary report

**Usage:**
```powershell
.\Invoke-IRCollect.ps1 -DriveLetter E -DaysBack 14
```

**Output:** External drive folder `IR_EVIDENCE_HOSTNAME_TIMESTAMP/` with:
- 01_volatile/ — running processes, connections, DNS cache
- 02_event_logs/ — all .evtx files
- 03_registry/ — hive exports
- 04_artifacts/ — hosts file, Defender logs, tasks, software
- 05_prefetch/ — .pf files showing program execution
- CHAIN_OF_CUSTODY_SHA256.txt — integrity verification
- IR_SUMMARY_REPORT.html — opens automatically in browser

**Time:** 20–40 minutes

---

### Invoke-IRRunbook.ps1
**Purpose:** Similar to Invoke-IRCollect.ps1 but stores results locally instead of external drive.

**Usage:**
```powershell
.\Invoke-IRRunbook.ps1 -DaysBack 7 -OutputRoot "C:\IREvidence"
```

---

### Invoke-LogAnalysis.ps1
**Purpose:** Automated analysis of collected logs using Chainsaw and Hayabusa.

**What it does:**
- Downloads Chainsaw + Hayabusa on first run
- Hunts through .evtx files for IOCs
- Generates attack timeline
- Flags suspicious events
- Creates HTML report

**Usage:**
```powershell
.\Invoke-LogAnalysis.ps1
```

**Output:** Desktop folder `IR_Analysis_TIMESTAMP/` with:
- Chainsaw_Results.csv — MITRE ATT&CK mapped detections
- Hayabusa_Timeline.csv — chronological event timeline
- IR_Analysis_Report.html

**Time:** 5–10 minutes (tools auto-download first time)

---

### Invoke-ExfilAnalysis.ps1
**Purpose:** Detects and quantifies data exfiltration.

**What it does:**
- Checks file access during breach window
- Resolves LNK shortcuts to see which files were opened
- Checks for archives created during breach
- Analyzes ScreenConnect transfer logs
- Generates exfiltration verdict

**Usage:**
```powershell
.\Invoke-ExfilAnalysis.ps1
```

**Output:** Desktop folder `Exfil_Analysis_TIMESTAMP/` with:
- ALERT_TaxFilesAccessed.csv
- ALERT_TaxFilesOpened.csv
- ALERT_Archives_BreachWindow.csv
- Exfil_Analysis_Report.html

**Time:** 10–15 minutes

---

### Remove-SuspectTools.ps1
**Purpose:** Safely removes unauthorized remote access tools.

**What it does:**
- Lists suspect tools before removing
- Uninstalls applications
- Stops and removes services
- Deletes startup items
- Generates removal log
- Creates before/after snapshots

**Usage:**
```powershell
# DRY RUN first
.\Remove-SuspectTools.ps1 -WhatIf

# Then execute
.\Remove-SuspectTools.ps1 -ConfirmedAttackerScreenConnect "f2b64c8c0b737dc1"
```

**Output:** Desktop files:
- RemovalBefore.csv — what was installed
- RemovalAfter.csv — what remains
- RemovalLog_TIMESTAMP.txt — detailed changes

**Time:** 5 minutes

---

### Invoke-TaxFirmFirewallHarden.ps1
**Purpose:** Implements best-practice firewall policies for tax/professional firms.

**What it does:**
- Backs up existing rules
- Sets default-deny inbound
- Allows essential ports (DNS, HTTPS, HTTP, NTP, email)
- Creates rules for ProSeries, QuickBooks, printers
- Opens IRS MeF e-file ports specifically
- Blocks known attacker ports
- Generates compliance report

**Usage:**
```powershell
.\Invoke-TaxFirmFirewallHarden.ps1 `
    -LegitimateScreenConnectID "bec82a294218ab1e" `
    -MSPSubnet "203.0.113.50/32"
```

**Output:**
- HTML compliance report
- Firewall rules applied
- Before/after policy comparison

**Time:** 5 minutes

**Test immediately after:**
```powershell
# Verify IRS e-file servers are reachable
Test-NetConnection efts.irs.gov -Port 443
Test-NetConnection la.www4.irs.gov -Port 443

# Verify MSP access still works
# (Have MSP test their ScreenConnect connection)
```

---

### Deploy-WazuhAgent.ps1
**Purpose:** Deploys Wazuh endpoint agent for centralized security monitoring.

**Prerequisites:**
- Wazuh manager already installed and running
- Manager IP address

**Usage:**
```powershell
.\Deploy-WazuhAgent.ps1 -ManagerIP "192.168.1.50" -AgentName "JL-W1"
```

**Output:**
- Wazuh agent installed and running
- Local agent status: `C:\Program Files\Wazuh\wazuh-agent.exe /s`

---

### wazuh-manager-install.sh
**Purpose:** Installs Wazuh SIEM manager on Ubuntu 22.04.

**Prerequisites:**
- Ubuntu 22.04 LTS VM with 4GB RAM, 40GB disk
- Internet access for package downloads

**Usage:**
```bash
sudo ./wazuh-manager-install.sh
```

**Output:**
- Wazuh manager running on port 443
- Default login: admin / changeme
- Web UI: https://wazuh-manager-ip

---

## Quick Start

### For a Fresh Breach Investigation

**Day 1 — Collect & Analyze:**
```powershell
# Step 1: Collect evidence
.\Invoke-IRCollect.ps1 -DriveLetter E -DaysBack 14

# Step 2: Analyze logs
.\Invoke-LogAnalysis.ps1

# Step 3: Check exfiltration
.\Invoke-ExfilAnalysis.ps1

# Step 4: Review HTML reports and CSV files
```

### For Removing Attacker Tools

```powershell
# Step 1: Preview what will be removed
.\Remove-SuspectTools.ps1 -WhatIf

# Step 2: Actually remove
.\Remove-SuspectTools.ps1

# Step 3: Verify removal (restart and recheck)
Get-WmiObject -Class Win32_Product | Where-Object Name -match "ScreenConnect|Splashtop"
```

### For Hardening After a Breach

```powershell
# Step 1: Harden firewall
.\Invoke-TaxFirmFirewallHarden.ps1 `
    -LegitimateScreenConnectID "bec82a294218ab1e" `
    -MSPSubnet "203.0.113.50/32"

# Step 2: Test critical applications
Test-NetConnection efts.irs.gov -Port 443  # IRS e-file
Get-NetTCPConnection -LocalPort 8019       # QuickBooks

# Step 3: Deploy Wazuh agent for monitoring
.\Deploy-WazuhAgent.ps1 -ManagerIP "192.168.1.50"
```

---

## Troubleshooting

### Script runs but produces no output
- Check execution policy: `Get-ExecutionPolicy`
- Set if needed: `Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass`
- Ensure running as Administrator

### Permission Denied errors
- Right-click PowerShell → Run as Administrator
- Some scripts need domain admin rights

### Tools not found (Chainsaw, Hayabusa)
- First run downloads tools automatically — may take 2–3 minutes
- Check internet connectivity
- Verify .NET 9 is installed: `dotnet --version`

### External drive not recognized
- Check drive letter: `Get-Volume`
- Ensure drive has sufficient free space (>10GB recommended)

---

## File Retention & Evidence Preservation

**Keep evidence for:**
- Criminal investigation: Indefinite (with law enforcement hold)
- Litigation: Duration of case + 3 years
- Regulatory audit: Duration of audit + 2 years
- Tax purposes: 7 years minimum

**Storage recommendations:**
- Write-once media (WORM) if available
- Cloud storage with immutability locks (AWS S3 Object Lock, Azure immutable blobs)
- Encrypted external drives stored in locked cabinet

---

**Last Updated:** May 2026  
**Script Version:** 1.0  
**Tested On:** Windows Server 2022, Windows 11 21H2+
