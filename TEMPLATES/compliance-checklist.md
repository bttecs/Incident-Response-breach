# Security Compliance Checklist

## Information Security Compliance Verification Checklist

**System/Project:** [Name]  
**Assessment Date:** [Date]  
**Assessed By:** [Name/Title]  
**Reviewed By:** [Name/Title]

---

## ACCESS CONTROL (AC)

### User Account Management
- [ ] **AC-2.1** Accounts created per authorization process
- [ ] **AC-2.2** Privileged account management implemented
- [ ] **AC-2.3** Shared/group account usage prohibited or controlled
- [ ] **AC-2.4** Account deprovisioning within 24 hours of termination
- [ ] **AC-2.5** Account review performed at least annually
- **Notes:** _______________

### Access Enforcement
- [ ] **AC-3.1** Access control mechanisms implemented per role/responsibility
- [ ] **AC-3.2** Principle of least privilege enforced
- [ ] **AC-3.3** Access denied by default (deny-all, allow-by-exception)
- [ ] **AC-3.4** Access control rules documented and maintained
- **Notes:** _______________

### Session Management
- [ ] **AC-11.1** Session lock enabled after inactivity (recommended 15 min)
- [ ] **AC-11.2** Session termination capability implemented
- [ ] **AC-11.3** Session timeout appropriately configured
- **Notes:** _______________

---

## IDENTIFICATION & AUTHENTICATION (IA)

### Authentication
- [ ] **IA-2.1** Authentication required for all system access
- [ ] **IA-2.2** Multi-factor authentication for privileged accounts
- [ ] **IA-2.3** Multi-factor authentication for remote access
- [ ] **IA-2.4** Authentication mechanisms appropriate to access level
- **Notes:** _______________

### Password Policy
- [ ] **IA-5.1** Minimum password length: 8 characters
- [ ] **IA-5.2** Password complexity required (upper, lower, number, special)
- [ ] **IA-5.3** Password history enforced (minimum 12 passwords)
- [ ] **IA-5.4** Password maximum age: 90 days
- [ ] **IA-5.5** Account lockout after failed attempts (5+)
- [ ] **IA-5.6** Temporary passwords required to change at first login
- **Notes:** _______________

---

## AUDIT & ACCOUNTABILITY (AU)

### Audit Logging
- [ ] **AU-2.1** Audit logging enabled for all systems
- [ ] **AU-2.2** Audit logging covers: logins, access, privilege changes, data access
- [ ] **AU-2.3** Sufficient audit detail for forensic analysis
- **Notes:** _______________

### Audit Review
- [ ] **AU-6.1** Audit logs reviewed at least weekly
- [ ] **AU-6.2** Security alerts monitored and responded to
- [ ] **AU-6.3** Suspicious activities investigated
- [ ] **AU-6.4** Audit events retained per record retention policy
- **Notes:** _______________

---

## SECURITY ASSESSMENT & AUTHORIZATION (CA)

### Risk Assessment
- [ ] **CA-2.1** Security categorization completed and documented
- [ ] **CA-2.2** Risk assessment conducted per NIST methodology
- [ ] **CA-2.3** Risk assessment reviewed and approved annually
- **Notes:** _______________

### Accreditation & Authorization
- [ ] **CA-6.1** Authorization to Operate (ATO) current and valid
- [ ] **CA-6.2** ATO conditions and constraints documented
- [ ] **CA-6.3** ATO review schedule established (typically annual)
- **Notes:** _______________

---

## CONFIGURATION MANAGEMENT (CM)

### Configuration Control
- [ ] **CM-1.1** Configuration management process documented
- [ ] **CM-1.2** Baseline configurations defined and documented
- [ ] **CM-1.3** Configuration changes follow change control process
- [ ] **CM-1.4** Configuration changes tested before deployment
- **Notes:** _______________

### Vulnerability Management
- [ ] **CM-3.1** Vulnerability scanning performed (automated tools)
- [ ] **CM-3.2** Scanning frequency appropriate to asset criticality (monthly minimum)
- [ ] **CM-3.3** Scan results reviewed and remediation planned
- [ ] **CM-3.4** Patch management process implemented
- **Notes:** _______________

---

## CONTINGENCY PLANNING (CP)

### Backup & Recovery
- [ ] **CP-9.1** Backup strategy documented and tested
- [ ] **CP-9.2** Backups performed at least weekly (daily for critical systems)
- [ ] **CP-9.3** Backup restoration tested at least annually
- [ ] **CP-9.4** Off-site backup copies maintained
- **Notes:** _______________

### Business Continuity
- [ ] **CP-2.1** Business Continuity Plan (BCP) developed and current
- [ ] **CP-2.2** Disaster Recovery Plan (DRP) developed and current
- [ ] **CP-2.3** BCP/DRP tested annually
- [ ] **CP-2.4** Recovery Time Objective (RTO) and Recovery Point Objective (RPO) defined
- **Notes:** _______________

---

## INCIDENT RESPONSE (IR)

### Incident Response Plan
- [ ] **IR-1.1** Incident Response Plan (IRP) documented
- [ ] **IR-1.2** Incident response roles and responsibilities defined
- [ ] **IR-1.3** Escalation procedures documented
- [ ] **IR-1.4** Key contact information maintained and current
- **Notes:** _______________

### Incident Handling
- [ ] **IR-6.1** Security incidents identified and reported
- [ ] **IR-6.2** Incidents logged and tracked
- [ ] **IR-6.3** Incident response process followed
- [ ] **IR-6.4** Lessons learned documented
- **Notes:** _______________

---

## MAINTENANCE (MA)

### System Maintenance
- [ ] **MA-1.1** Maintenance procedures documented
- [ ] **MA-1.2** Maintenance windows scheduled and controlled
- [ ] **MA-1.3** Maintenance activity logged and tracked
- [ ] **MA-1.4** Remote maintenance uses secure protocols (SSH, VPN, MFA)
- **Notes:** _______________

---

## PHYSICAL & ENVIRONMENTAL PROTECTION (PE)

### Physical Access
- [ ] **PE-2.1** Physical access controls restrict entry to secure areas
- [ ] **PE-2.2** Access badges/credentials required
- [ ] **PE-2.3** Visitor access logged and supervised
- [ ] **PE-2.4** Access logs reviewed periodically
- **Notes:** _______________

### Environmental Controls
- [ ] **PE-11.1** HVAC systems maintain appropriate temperature/humidity
- [ ] **PE-11.2** Fire suppression systems in place (not water/halon in server rooms)
- [ ] **PE-11.3** Environmental monitoring enabled
- **Notes:** _______________

---

## PLANNING (PL)

### Security Planning
- [ ] **PL-1.1** System Security Plan (SSP) developed and maintained
- [ ] **PL-1.2** SSP reviewed and updated annually or after significant changes
- [ ] **PL-1.3** Security requirements documented
- **Notes:** _______________

---

## PERSONNEL SECURITY (PS)

### Personnel Management
- [ ] **PS-3.1** Background investigation completed before access granted
- [ ] **PS-3.2** Periodic security reviews conducted
- [ ] **PS-3.3** Security training conducted annually
- **Notes:** _______________

### Termination
- [ ] **PS-8.1** Termination procedures document account/access removal
- [ ] **PS-8.2** System access disabled within 24 hours of termination
- [ ] **PS-8.3** Building access revoked
- [ ] **PS-8.4** Equipment returned and checked
- **Notes:** _______________

---

## RISK ASSESSMENT (RA)

### Risk Assessment Process
- [ ] **RA-3.1** Risk assessments conducted at least annually
- [ ] **RA-3.2** Risk register maintained and current
- [ ] **RA-3.3** New/changed systems assessed for risk
- [ ] **RA-3.4** Risk findings documented and tracked
- **Notes:** _______________

---

## SYSTEM & COMMUNICATIONS PROTECTION (SC)

### Cryptography
- [ ] **SC-7.1** Encryption in transit for sensitive data
- [ ] **SC-7.2** TLS 1.2+ for web/network communications
- [ ] **SC-7.3** Encryption at rest for sensitive data at rest
- [ ] **SC-7.4** Key management procedures implemented
- **Notes:** _______________

### Network Security
- [ ] **SC-8.1** Network segmentation implemented
- [ ] **SC-8.2** Firewall rules documented and reviewed
- [ ] **SC-8.3** DMZ implemented for public-facing systems
- [ ] **SC-8.4** Intrusion detection/prevention systems deployed
- **Notes:** _______________

---

## SYSTEM & INFORMATION INTEGRITY (SI)

### Malware Protection
- [ ] **SI-3.1** Anti-malware tools deployed on end-user devices
- [ ] **SI-3.2** Anti-malware definitions updated automatically
- [ ] **SI-3.3** Real-time scanning enabled
- **Notes:** _______________

### Security Monitoring
- [ ] **SI-4.1** System monitoring enabled on production systems
- [ ] **SI-4.2** SIEM or log aggregation tool deployed
- [ ] **SI-4.3** Security events monitored in near real-time
- **Notes:** _______________

---

## NISPOM COMPLIANCE (if applicable)

### Classification & Marking
- [ ] Information classified per Executive Order or DoD guidance
- [ ] Classified information marked appropriately
- [ ] Classification markings used on all classified documents
- **Notes:** _______________

### Personnel Security
- [ ] Security clearances verified for all personnel with classified access
- [ ] Personnel with expired clearances have access removed
- [ ] Periodic reinvestigations scheduled per clearance level
- **Notes:** _______________

### Physical Security (NISPOM)
- [ ] Classified material stored in approved containers/areas
- [ ] Facilities meet NISPOM requirements for classified access
- [ ] Visitor management procedures comply with NISPOM
- **Notes:** _______________

---

## OVERALL COMPLIANCE ASSESSMENT

### Compliance Score
| Area | Compliant | Partial | Non-Compliant | Score |
|------|-----------|---------|----------------|-------|
| Access Control | [ ] | [ ] | [ ] | [%] |
| Authentication | [ ] | [ ] | [ ] | [%] |
| Audit & Accountability | [ ] | [ ] | [ ] | [%] |
| Risk Management | [ ] | [ ] | [ ] | [%] |
| Vulnerability Management | [ ] | [ ] | [ ] | [%] |
| Incident Response | [ ] | [ ] | [ ] | [%] |
| Physical Security | [ ] | [ ] | [ ] | [%] |
| **OVERALL** | | | | **[%]** |

### Compliance Status
[ ] **Compliant** (≥95% compliance)  
[ ] **Compliant with Conditions** (90-95% compliance)  
[ ] **Partially Compliant** (75-90% compliance)  
[ ] **Non-Compliant** (<75% compliance)

### Gaps Identified
| Gap | Severity | Remediation | Owner | Target Date |
|-----|----------|-------------|-------|------------|
| [Gap] | [H/M/L] | [Action] | [Owner] | [Date] |

### Next Steps
1. [Action item]
2. [Action item]
3. [Action item]

---

## Sign-Off

| Role | Name | Signature | Date |
|------|------|-----------|------|
| Assessor | [Name] | __________ | _____ |
| Reviewer | [Name] | __________ | _____ |

---

**Assessment Date:** [Date]  
**Next Review Date:** [Date + 12 months]
