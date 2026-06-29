# Phase 1: CONTAIN — Immediate Response (Hours 0–2)

## Objective
Stop the bleeding. Isolate the breach, prevent further data exfiltration, preserve evidence, and establish a forensic baseline.

---

## Step-by-Step Actions

### 1. Identify Affected Systems (First 10 minutes)
**What to ask:**
- When was the breach first noticed?
- Which user/machine first showed signs?
- What unusual activity triggered the alert? (slow performance, strange processes, network traffic spike)
- Is this affecting just one machine or the entire network?

**How to find it quickly:**
```powershell
# Remote machines (from admin workstation)
Get-Process "ScreenConnect","Splashtop","TeamViewer" -ComputerName * -EA SilentlyContinue

# Check for suspicious scheduled tasks
Get-ScheduledTask -TaskName "*atera*","*splashtop*" -CimSession (Get-ADComputer -Filter *).Name -EA SilentlyContinue

# Recent logins from external IP
Get-WinEvent -LogName Security -FilterXPath "*[System[EventID=4624 and TimeCreated[@SystemTime>='$(([datetime]::UtcNow.AddHours(-24)).ToUniversalTime().ToString('o'))']]]" | 
    Where-Object {$_.Properties[18].Value -notmatch "127.0.0.1|LOCAL"} | Select-Object TimeCreated, @{N='Username';E={$_.Properties[5].Value}}, @{N='Source IP';E={$_.Properties[18].Value}}
```

**Document:**
- [ ] Machine name / IP address
- [ ] Operating system (Windows Server 2022? Win 11?)
- [ ] Who normally uses it? (user account)
- [ ] What sensitive data does it hold? (tax files? accounting? HR records?)
- [ ] Is it on the domain or workgroup?

---

### 2. Notify the Incident Response Team (Immediately)
**Contact list (fill in your details):**
- [ ] CISO / Security Officer: ________________
- [ ] IT Manager: ________________
- [ ] Legal Counsel: ________________
- [ ] HR Director (if employee breach): ________________
- [ ] CFO (if financial data): ________________
- [ ] Customer account manager (if customer data): ________________

**What to tell them:**
```
Subject: SECURITY INCIDENT — [System Name] Compromised

Timeline:
- Discovery time: [Date/Time]
- Breach likely occurred: [Date range if known]
- Affected systems: [Machine names]
- Affected data: [Types - tax files, SSNs, financial records]

Immediate actions taken:
- Isolated machine from network
- Disabled attacker accounts
- Started forensic collection

Next steps:
- Full forensic investigation (24 hours)
- Determine scope of data exposure
- Breach notification (if required by law)

No customer communication until we confirm what was stolen.
```

---

### 3. Collect Volatile Data (Before ANY Reboots)
**CRITICAL:** This step must happen before the machine is rebooted, shut down, or subjected to any antivirus scans.

**Why:** RAM contains:
- Attacker's running processes and commands
- Active network connections (where they connected FROM)
- Decryption keys and in-memory passwords
- Malware that only exists in RAM

**Rebooting destroys all of this permanently.**

#### 3a — Run the Evidence Collection Script
```powershell
# On a USB drive or network share, prepare:
# 1. An external hard drive (8GB+ free space)
# 2. Windows PowerShell (admin rights required)

# Option 1: Using Invoke-IRCollect.ps1 (Fully automated)
# Right-click PowerShell → Run as Administrator
.\Invoke-IRCollect.ps1 -DriveLetter E -DaysBack 14

# Option 2: Manual collection (if script not available)
# See manual steps below
```

#### 3b — Manual Evidence Collection (If Script Unavailable)
```powershell
# Run as Administrator
# Estimated time: 15–20 minutes

$DRIVE = "E:"  # Change to your external drive letter
$EVIDENCE = "$DRIVE\IR_Evidence_$(Get-Date -Format yyyyMMdd_HHmm)"
New-Item -ItemType Directory $EVIDENCE -Force | Out-Null

# 1. MEMORY DUMP
Write-Host "Collecting memory..." -ForegroundColor Yellow
# Use these tools in order of preference:
# Option A: Windows 11/Server 2022 native (fastest)
.\Psanaly.exe -dump $EVIDENCE\memory.dmp

# Option B: DumpIt.exe (if available)
.\DumpIt.exe

# Option C: Fallback — document what's in memory
Get-Process | Export-Csv "$EVIDENCE\running_processes.csv" -NoTypeInformation

# 2. NETWORK CONNECTIONS
Write-Host "Collecting network state..." -ForegroundColor Yellow
Get-NetTCPConnection -State Established | Export-Csv "$EVIDENCE\tcp_connections.csv" -NoTypeInformation
Get-DnsClientCache | Export-Csv "$EVIDENCE\dns_cache.csv" -NoTypeInformation
arp -a | Out-File "$EVIDENCE\arp_table.txt"
route print | Out-File "$EVIDENCE\routing_table.txt"

# 3. USER SESSIONS
Write-Host "Collecting user sessions..." -ForegroundColor Yellow
quser | Out-File "$EVIDENCE\logged_in_users.txt"
Get-LocalUser | Export-Csv "$EVIDENCE\local_users.csv" -NoTypeInformation
Get-LocalGroupMember Administrators | Export-Csv "$EVIDENCE\administrators.csv" -NoTypeInformation

# 4. RUNNING SERVICES
Write-Host "Collecting services..." -ForegroundColor Yellow
Get-Service | Export-Csv "$EVIDENCE\services.csv" -NoTypeInformation
Get-WmiObject -Class Win32_Service | Export-Csv "$EVIDENCE\services_detailed.csv" -NoTypeInformation

# 5. EVENT LOGS (last 7 days)
Write-Host "Collecting event logs..." -ForegroundColor Yellow
Get-WinEvent -LogName Security -MaxEvents 10000 | Export-Csv "$EVIDENCE\Security_7days.csv" -NoTypeInformation
Get-WinEvent -LogName System -MaxEvents 10000 | Export-Csv "$EVIDENCE\System_7days.csv" -NoTypeInformation

Write-Host "Evidence collected to: $EVIDENCE" -ForegroundColor Green
Write-Host "DO NOT RESTART THE MACHINE YET" -ForegroundColor Red
```

---

### 4. Disable Attacker Accounts (Immediately)
**Why:** Stop the attacker from continuing to access the system using compromised credentials.

#### 4a — Identify Attacker Accounts
```powershell
# Look for accounts created recently or with suspicious activity
Get-LocalUser | Where-Object {$_.LastLogonDate -ge (Get-Date).AddDays(-14)} | Select-Object Name, Enabled, LastLogonDate

# Check for privileged accounts
Get-LocalGroupMember Administrators

# Look for accounts with no password expiration
Get-LocalUser | Where-Object {$_.PasswordExpires -eq $null}
```

#### 4b — Disable Attacker Accounts
```powershell
# DISABLE any suspicious accounts immediately
# Example: if "attacker_user" was created on May 15 during the breach
Disable-LocalUser -Name "attacker_user"

# Change the password for accounts YOU control
Set-LocalUser -Name "pro" -Password (Read-Host -AsSecureString -Prompt "Enter new password")

# For DOMAIN accounts (if on Active Directory)
Disable-ADUser -Identity "suspicious.user"
Set-ADUser -Identity "pro.JLTAX" -ChangePasswordAtLogon $true
```

**Document:**
- [ ] Which accounts were disabled?
- [ ] When were they disabled?
- [ ] Who disabled them? (for audit trail)

---

### 5. Block Attacker IP Addresses at Firewall (Now)
**Why:** Stop the attacker from reconnecting to the machine.

#### 5a — Identify Attacker IPs from Event Logs
```powershell
# Failed login attempts from external IPs
Get-WinEvent -LogName Security -FilterXPath "*[System[EventID=4625]]" -MaxEvents 1000 |
    ForEach-Object {
        $event = [xml]$_.ToXml()
        [PSCustomObject]@{
            TimeCreated = $_.TimeCreated
            SourceIP = $event.Event.EventData.Data[19].'#text'
            Account = $event.Event.EventData.Data[5].'#text'
            Reason = $event.Event.EventData.Data[8].'#text'
        }
    } |
    Where-Object {$_.SourceIP -notmatch "127.0.0.1|192.168|10.0"} |
    Select-Object -First 20 | Format-Table -AutoSize
```

#### 5b — Block IPs at Firewall
**Via PowerShell:**
```powershell
# Block inbound from attacker IP addresses
$attackerIPs = @("203.0.113.50", "198.51.100.75")  # Replace with actual IPs

foreach ($ip in $attackerIPs) {
    New-NetFirewallRule `
        -DisplayName "Block Attacker IP $ip" `
        -Direction Inbound `
        -Action Block `
        -RemoteAddress $ip `
        -Protocol Any `
        -Enabled True | Out-Null
    Write-Host "Blocked: $ip" -ForegroundColor Red
}
```

**Via GUI:**
1. Open Windows Firewall with Advanced Security (wf.msc)
2. Right-click "Inbound Rules" → "New Rule..."
3. Rule Type: "Custom"
4. Protocol and Ports: "All"
5. Direction: "Inbound"
6. Action: "Block"
7. Remote IP Address: Select "These IP addresses" → Add each attacker IP
8. Click Finish

**Document:**
- [ ] Which IP addresses were blocked?
- [ ] Where did you get these IPs? (firewall log, event logs, etc.)

---

### 6. Uninstall Unauthorized Remote Access Tools (Do This Now)
**Why:** Remove the attacker's backdoor before they reconnect.

#### 6a — Identify Unauthorized RMM Tools
Common suspicious tools:
- ScreenConnect (session IDs: check if there are multiple)
- Splashtop
- Advanced Monitoring Agent
- Atera
- ScriptRunner (N-able)
- TeamViewer (if not authorized)
- AnyDesk (if not authorized)
- Chrome Remote Desktop (if not authorized)

```powershell
# List all installed programs
Get-WmiObject -Class Win32_Product |
    Where-Object {$_.Name -match "ScreenConnect|Splashtop|Atera|ScriptRunner|TeamViewer|AnyDesk|Remote"} |
    Select-Object Name, Version, InstallDate, Vendor
```

#### 6b — Use Remove-SuspectTools.ps1 (Automated)
```powershell
# Right-click PowerShell → Run as Administrator

# DRY RUN first (see what would be removed without touching anything)
.\Remove-SuspectTools.ps1 -WhatIf

# Then actually remove (after confirming with management/MSP)
.\Remove-SuspectTools.ps1 -ConfirmedAttackerScreenConnect "f2b64c8c0b737dc1"
```

#### 6c — Manual Removal
```powershell
# Example: Uninstall Splashtop
$splashtop = Get-WmiObject -Class Win32_Product | Where-Object {$_.Name -like "*Splashtop*"}
if ($splashtop) {
    Write-Host "Uninstalling $($splashtop.Name)..." -ForegroundColor Yellow
    $splashtop.Uninstall()
}

# Stop and remove services
Stop-Service -Name "SplashtopSvc" -Force -EA SilentlyContinue
Remove-Service -Name "SplashtopSvc" -EA SilentlyContinue
```

**Document:**
- [ ] Which tools were removed?
- [ ] When were they removed?
- [ ] Did removal succeed or fail?
- [ ] Verify they're gone: restart and check `Get-WmiObject -Class Win32_Product` again

---

### 7. Preserve Evidence to External Drive (Do This Now)
**Why:** Chain of custody — prove that what you found is what you analyzed, and nobody tampered with it.

```powershell
# If using Invoke-IRCollect.ps1, this is automatic
# If manual collection, do this:

$EVIDENCE = "E:\IR_Evidence_*"
$EVIDENCE_PATH = (Resolve-Path $EVIDENCE | Select-Object -Last 1).Path

# Calculate SHA-256 hash of every file (proves integrity)
Get-ChildItem $EVIDENCE_PATH -Recurse -File |
    ForEach-Object {
        $hash = (Get-FileHash $_.FullName -Algorithm SHA256).Hash
        "$($hash) `t$($_.FullName)" 
    } | Out-File "$EVIDENCE_PATH\CHAIN_OF_CUSTODY_SHA256.txt"

Write-Host "Hash manifest created: CHAIN_OF_CUSTODY_SHA256.txt" -ForegroundColor Green
Write-Host "This proves the evidence has not been tampered with." -ForegroundColor Green
```

**Document:**
- [ ] Hash manifest file created and saved
- [ ] External drive physically labeled with case number and date
- [ ] Chain of custody form signed by who collected evidence

---

### 8. Alert Executive Leadership (Before End of Business)
**What NOT to say on email or Teams:**
- ❌ Specific vulnerability details
- ❌ Attacker nicknames or motivations
- ❌ "We've been hacked and don't know what to do"

**What TO say:**
```
SUBJECT: [URGENT] Security Incident — Status Update

An unauthorized actor has accessed [System Name]. We have:

✓ Isolated the affected system
✓ Collected forensic evidence
✓ Disabled attacker-created accounts
✓ Blocked attacker IP addresses
✓ Begun formal investigation

Expected timeline:
- Full forensic analysis: 24 hours
- Eradication: 24–48 hours
- Recovery: 3–5 days
- Remediation: 2–3 weeks

We will provide an update by [specific time tomorrow].
No customer communication until investigation is complete.
```

---

## Phase 1 Checklist — Print This

```
CONTAIN PHASE CHECKLIST
═════════════════════════════════════════════════════

☐ Affected systems identified & documented
☐ Incident response team notified
☐ Volatile data collected to external drive
☐ Attacker accounts identified
☐ Attacker accounts disabled
☐ Attacker account passwords changed
☐ Attacker IP addresses blocked at firewall
☐ Unauthorized RMM tools uninstalled
☐ Chain of custody documentation created
☐ Executive leadership notified (no technical details)
☐ Evidence secured on external drive
☐ Machine NOT restarted yet

Time started: _____________________
Time completed: __________________
Completed by: _____________________
Manager approval: _________________
```

---

## What NOT to Do in Phase 1

❌ **Do NOT power off the machine** — destroys RAM evidence  
❌ **Do NOT run antivirus scans** — may clear malware before evidence collection  
❌ **Do NOT let the user restart their machine** — they might click "Install Updates"  
❌ **Do NOT change passwords through email** — attacker watches the inbox  
❌ **Do NOT post on social media** — notification must be controlled and legal  
❌ **Do NOT call the police immediately** — call legal first (they'll advise)  
❌ **Do NOT destroy evidence** — everything collected needs to be preserved  

---

## After Phase 1 is Complete

When all items on the checklist are done:
1. Move evidence drive to secure location
2. Proceed to **PHASE_2_INVESTIGATE.md**
3. Begin forensic analysis on a CLEAN machine
4. Do NOT reconnect compromised machine to network

**Time to Phase 2:** Immediate (can run in parallel with continued containment)

---

**Phase 1 Duration:** 0–2 hours  
**Success Criteria:** Breach contained, evidence preserved, attacker access stopped  
**Next Document:** PHASE_2_INVESTIGATE.md
