# Security Frameworks & Standards Reference

## Overview

This document provides guidance on the key security frameworks and government standards that govern Product Security Analyst responsibilities.

---

## Risk Management Framework (RMF)

### Purpose
RMF is a systematic, documented, and repeatable process for security categorization, control selection, implementation, assessment, authorization, and continuous monitoring of information systems.

### RMF Process Steps

#### 1. Categorize (Step 1)
**Objective:** Determine security categorization of the information system

**Activities:**
- Identify information types and their sensitivity/criticality
- Determine impact levels (Low, Moderate, High) per FIPS 199
- Document system categorization per FIPS 200
- Obtain approval from Authorizing Official (AO)

**Output:**
- Security Categorization Report
- Impact Level determination

#### 2. Select Controls (Step 2)
**Objective:** Choose security controls appropriate for system categorization

**Activities:**
- Review baseline controls from NIST SP 800-53
- Tailor controls to organizational context
- Identify control enhancements needed
- Document control selection and rationale

**Considerations:**
- Risk assessment findings
- System requirements and architecture
- Organizational policy
- Industry best practices

**Output:**
- Security Control Plan (SCP)
- Controls and Enhancements documentation

#### 3. Implement Controls (Step 3)
**Objective:** Implement selected security controls in the system

**Activities:**
- Develop Control Implementation Plan (CIP)
- Engineer security measures into system design
- Configure security tools and settings
- Establish security procedures and training
- Conduct security design reviews

**Responsibility:**
- Security Analyst provides oversight and validation
- Engineering/IT Operations perform implementation
- Testing team validates control functionality

**Output:**
- Implemented security controls
- Control implementation documentation
- Test results and validation reports

#### 4. Assess Controls (Step 4)
**Objective:** Determine control effectiveness and compliance

**Activities:**
- Develop Assessment and Authorization (A&A) Plan
- Conduct security assessment per NIST SP 800-53A
- Test control implementation and effectiveness
- Document assessment findings
- Identify deficiencies and non-compliances

**Assessment Types:**
- Document review
- Interviews and questionnaires
- Technical testing
- Observation and inspection
- Automated scanning

**Output:**
- Assessment Report
- Control effectiveness determination
- List of deficiencies and remediation requirements

#### 5. Authorize System (Step 5)
**Objective:** Obtain formal authorization to operate the system

**Activities:**
- Prepare Authorization Package (includes assessment report, remediation plan)
- Present findings to Authorizing Official
- Risk Acceptance memo (for any residual risk)
- Formal Accreditation decision
- Establish conditions for continued operation

**Authorization Decisions:**
- **Authorize to Operate (ATO):** System may operate with identified conditions
- **Authorize to Operate Contingently:** Operates under specific constraints
- **Denial of Authorization:** System cannot operate until issues resolved

**Output:**
- Authorization Decision memo
- Risk Acceptance memo (if applicable)
- Conditions for continued operation

#### 6. Monitor & Maintain (Step 6)
**Objective:** Continuously verify control effectiveness and compliance

**Activities:**
- Establish continuous monitoring strategy
- Monitor security control compliance
- Perform periodic assessments (annual minimum)
- Update controls as needed
- Report compliance status

**Monitoring Activities:**
- Automated tool monitoring (SIEM, vulnerability scanners)
- Manual reviews and spot checks
- Configuration management verification
- Patch and update validation
- User access reviews
- Security incident analysis

**Output:**
- Continuous Monitoring Plan
- Monitoring reports
- Updated Assessment Report (annually)
- Corrective action plans for deficiencies

### RMF Timeline

Typical RMF process timeline:
- **Categorize:** 2-4 weeks
- **Select:** 4-6 weeks
- **Implement:** 4-12 weeks (depending on system complexity)
- **Assess:** 2-4 weeks
- **Authorize:** 1-2 weeks
- **Monitor:** Ongoing (formal reviews annually)

**Total Initial Authorization:** 13-30 weeks depending on system complexity

### RMF Documentation

Key RMF documents:
1. **Security Categorization Report** — System impact level
2. **Security Control Plan** — Selected controls and rationale
3. **Control Implementation Plan** — Implementation details
4. **Assessment and Authorization Plan** — A&A approach
5. **Assessment Report** — Testing results and findings
6. **System Security Plan (SSP)** — Complete system security documentation
7. **Authorization Package** — ATO submission package
8. **Continuous Monitoring Plan** — Ongoing compliance strategy
9. **Rules of Behavior** — User security responsibilities

---

## NISPOM (National Industrial Security Program Operating Manual)

### Purpose
NISPOM establishes security requirements for contractors with access to classified information.

### Key Areas

#### 1. Classification Controls
- Proper classification of information
- Classification markings and handling
- Downgrading and declassification procedures
- Access to classified information

#### 2. Personnel Security
- Background investigations
- Clearance requirements (Secret, Top Secret, TS/SCI)
- Need-to-know determination
- Termination and clearance revocation procedures

#### 3. Physical Security
- Secure facilities and areas
- Access controls and badge systems
- Visitor management
- Equipment and material accountability
- Destruction of classified material

#### 4. Information Security
- System security plans
- Encryption requirements
- Storage security (safes, secure containers)
- Transportation security
- Remote access controls

#### 5. Counterintelligence
- Threat awareness
- Foreign influence identification
- Incident reporting
- Counterintelligence support

#### 6. Facility Security Officer (FSO)
- Responsible for NISPOM program implementation
- Security training and awareness
- Incident investigation and reporting
- Compliance monitoring

### NISPOM Compliance for Security Analyst
- Support Facility Security Officer in policy implementation
- Integrate NISPOM requirements into security architecture
- Audit compliance with classification and handling procedures
- Ensure clearance and need-to-know verification
- Coordinate on facility and physical security requirements
- Support counterintelligence awareness programs

---

## ICD 503: Security Categorization and Control Selection

### Purpose
Intelligence Community Directive (ICD) 503 implements RMF within the IC, with specific focus on intelligence sources and methods protection.

### Key Differences from NIST RMF
- **Categorization:** Uses IC-specific impact categories
- **Controls:** Includes IC-specific control requirements
- **Intelligence Protection:** Emphasis on protecting sources and methods
- **Markings:** Uses IC classification markings (TS//SCI, etc.)
- **Assessment:** IC-specific assessment criteria

### ICD 503 for Security Analyst
- Follow ICD 503 RMF process for IC systems
- Use IC-specific control baselines
- Apply IC classification and compartmentation rules
- Coordinate with IC Security community
- Document per IC security requirements

---

## DoD Overprint to NISPOM

### Purpose
DoD-specific security requirements that supplement NISPOM for Department of Defense contractors.

### Key Areas
- **System Security Planning:** DoD-specific system categorization
- **Defense Counterintelligence and Security Agency (DCSA):** Oversight authority
- **Adjudication:** DoD-specific clearance adjudication
- **Cybersecurity Requirements:** DoD Cybersecurity Maturity Model Certification (CMMC)
- **Supply Chain Risk Management (SCRM):** Vendor security requirements

### CMMC (Cybersecurity Maturity Model Certification)
DoD contractors must meet CMMC levels based on contract requirements:
- **Level 1:** Basic cyber hygiene
- **Level 2:** Intermediate practices
- **Level 3:** Advanced/optimized practices

### DoD Requirements for Security Analyst
- Integrate DoD RMF requirements into security design
- Support DCSA oversight and compliance
- Implement CMMC controls per contract requirements
- Manage supply chain security assessments
- Track DoD compliance metrics

---

## NIST Cybersecurity Framework

### Core Functions
1. **Identify** — Understand assets, risks, and vulnerabilities
2. **Protect** — Implement safeguards
3. **Detect** — Identify and analyze anomalies/incidents
4. **Respond** — Contain and remediate incidents
5. **Recover** — Restore normal operations

### NIST SP 800-53 — Security and Privacy Controls

**Control Categories (A-S):**
- **AC** — Access Control
- **AT** — Awareness and Training
- **AU** — Audit and Accountability
- **CA** — Security Assessment and Authorization
- **CM** — Configuration Management
- **CP** — Contingency Planning
- **IA** — Identification and Authentication
- **IR** — Incident Response
- **MA** — Maintenance
- **MP** — Media Protection
- **PE** — Physical and Environmental Protection
- **PL** — Planning
- **PS** — Personnel Security
- **RA** — Risk Assessment
- **SA** — System and Services Acquisition
- **SC** — System and Communications Protection
- **SI** — System and Information Integrity

### NIST SP 800-30 — Risk Assessment

Standard risk assessment methodology:
1. **Prepare for Assessment** — Define scope, gather information
2. **Conduct Assessment** — Identify threats, vulnerabilities, likelihood, impact
3. **Communicate Results** — Document findings and recommendations
4. **Maintain Assessment** — Update as systems change

### NIST Impact Levels (FIPS 199)

| Confidentiality | Integrity | Availability | Impact Level |
|---|---|---|---|
| Low | Low | Low | **Low** |
| Low | Low | Moderate | **Moderate** |
| Low | Moderate | Low | **Moderate** |
| Other combinations involving Moderate/High | Various | **Moderate** |
| High | High | High (or any High) | **High** |

---

## ISO/IEC 27001 — Information Security Management

### Scope
Framework for establishing, implementing, maintaining, and improving information security management system (ISMS).

### Key Requirements
- Information asset identification
- Risk assessment and treatment
- Control implementation
- Monitoring and review
- Continuous improvement

### ISO 27001 vs. NIST RMF
- ISO focuses on management system and continuous improvement
- NIST focuses on system categorization and control selection
- Both can be used complementarily

---

## Regulatory Compliance Timeline

### Typical Compliance Assessment Schedule
- **Quarterly:** Risk register reviews, compliance metrics update
- **Semi-annually:** Vulnerability assessment update
- **Annually:** Full compliance assessment, RMF review
- **As needed:** After system changes, following security incidents, in response to threats

### Audit Schedule
- **Internal audits:** Annually (minimum)
- **External audits:** Per contract/regulatory requirement (annually to every 3 years)
- **Regulatory audits:** Per regulatory body (NSA, DCSA, etc.)

---

## Compliance Mapping Example

### Example: Secure Access Control

| NIST 800-53 | ISO 27001 | NISPOM | DoD CMMC |
|---|---|---|---|
| **AC-2** Account Management | A.9.2.1 User registration and de-registration | Classification access procedures | AC.1.002 Access Control |
| **AC-3** Access Enforcement | A.9.1.2 User access management | Need-to-know procedures | AC.2.002 Access approval |
| **AC-4** Information Flow Enforcement | A.13.1.1 Network security | Secure communications | SC.2.003 Secure remote access |

---

## Key Compliance Documents to Maintain

1. **Security Categorization Report**
2. **Risk Assessment Report**
3. **Security Control Plan**
4. **System Security Plan (SSP)**
5. **Assessment Report**
6. **Authorization Package**
7. **Continuous Monitoring Plan**
8. **Policies and Procedures**
9. **Training and Awareness Records**
10. **Incident Response Documentation**
11. **Audit and Assessment Results**
12. **Corrective Action Plans**

---

## Revision History

| Version | Date | Changes | Author |
|---------|------|---------|--------|
| 1.0 | [Date] | Initial frameworks reference | [Author] |

---

**Document Owner:** [Insert Title]  
**Last Updated:** [Date]  
**Next Review Date:** [Date + 12 months]
