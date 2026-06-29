# Security Assessment Report Template

## Security Assessment Report

**System/Project Name:** [System Name]  
**Assessment Date(s):** [Start] – [End]  
**Report Date:** [Date]  
**Assessment Type:** [Initial / Periodic / Compliance / Threat-based]  
**Assessor(s):** [Names/Titles]  
**Approved By:** [Name/Title]

---

## Executive Summary

[1-2 paragraph high-level summary of assessment findings, overall risk posture, and key recommendations. Suitable for executive audience.]

### Assessment Scope
- **System Boundaries:** [Describe what was and wasn't included]
- **Timeframe:** [Assessment dates]
- **Assessment Methodology:** [NIST SP 800-53A / OWASP / Custom / etc.]
- **Systems/Controls Assessed:** [List major areas]

### Overall Assessment Results
- **Controls Tested:** [#]
- **Controls Compliant:** [#] ([%])
- **Controls Non-Compliant:** [#] ([%])
- **Controls Unable to Assess:** [#]
- **Overall Risk Posture:** [Acceptable / Acceptable with Conditions / Unacceptable]

---

## Key Findings

### Critical Findings
[List each critical finding]

**Finding ID:** C-001  
**Title:** [Title]  
**Description:** [Description of issue]  
**Impact:** [Business/security impact if not remediated]  
**Recommended Remediation:** [Specific actions]

### High Findings
[List each high finding]

### Medium Findings
[Summary or list if numerous]

### Low Findings
[Summary only or reference supporting document]

---

## Assessment Methodology

**Framework:** [NIST RMF / NIST SP 800-53 / ISO 27001 / etc.]  
**Assessment Approach:** [Desk review / Interviews / Technical testing / Observation]  
**Standards Used:** [List applicable standards]

---

## Detailed Assessment Results

### Control Assessment Summary

| Control ID | Control Name | Assessment Status | Compliant? | Notes |
|------------|--------------|-------------------|-----------|-------|
| AC-2 | Account Management | Reviewed | Yes | [Notes] |
| AC-3 | Access Enforcement | Tested | Partial | [Notes] |

### By Domain/Functional Area

#### 1. Access Control (AC)
[Assessment results for access control controls]
- **Compliant Controls:** [List]
- **Non-Compliant Controls:** [List]
- **Key Issues:** [Summary]

#### 2. Audit and Accountability (AU)
[Continue for each domain...]

#### 3. [Other Domains]
[Continue...]

---

## Assessment Evidence

**Evidence Collection Methods:**
- Document review (policies, procedures, configuration)
- Technical testing (scans, testing, inspection)
- Interviews with [Roles]
- Observation of [Processes]
- Automated assessment tools

**Key Documents Reviewed:**
- [System Security Plan]
- [Control Implementation Plans]
- [Policies and Procedures]
- [Previous Assessment Reports]

---

## Risk Analysis

### Risk Summary
| Risk Level | Count | Percentage |
|-----------|-------|-----------|
| Critical | [#] | [%] |
| High | [#] | [%] |
| Medium | [#] | [%] |
| Low | [#] | [%] |
| **Total** | [#] | 100% |

### Risk by Category
- **Technical:** [#] risks
- **Operational:** [#] risks
- **Compliance:** [#] risks

---

## Recommendations

### Priority 1 (Critical) — Immediate Action Required
1. [Recommendation 1]
2. [Recommendation 2]

### Priority 2 (High) — Remediate within 30 days
1. [Recommendation]
2. [Recommendation]

### Priority 3 (Medium) — Remediate within 60-90 days
1. [Recommendation]

### Priority 4 (Low) — Consider for future remediation
1. [Recommendation]

---

## Remediation Plan

### Remediation Status Summary
| Priority | # Findings | Estimated Completion | Owner |
|----------|-----------|---------------------|-------|
| Critical | [#] | [Date] | [Title] |
| High | [#] | [Date] | [Title] |
| Medium | [#] | [Date] | [Title] |
| Low | [#] | [Date] | [Title] |

### Detailed Remediation Plan
[Insert detailed remediation plan with timeline, owners, and success criteria - or reference separate document]

---

## Conclusion

[Summary of assessment results, overall security posture determination, and recommended next steps.]

**Assessment Conclusion:** 
- [ ] System is compliant and ready for authorization
- [ ] System compliant with identified conditions for authorization
- [ ] System requires remediation before authorization
- [ ] Further assessment needed before determination

---

## Appendices

- **A.** Detailed Findings and Evidence
- **B.** Control Assessment Detail
- **C.** Interview Notes
- **D.** Technical Scan Results
- **E.** Policy and Procedure References
- **F.** System Architecture Diagrams
- **G.** Glossary and Abbreviations

---

## Document Information

| Item | Value |
|------|-------|
| **Report Date** | [Date] |
| **Next Review Date** | [Date] |
| **Classification** | [Classification level if applicable] |
| **Distribution** | [Intended recipients] |
| **Retention Period** | [Period per records management policy] |

---

**Assessor Signature:** _________________________ **Date:** _______  
**Reviewer Signature:** _________________________ **Date:** _______  
**Approval Signature:** _________________________ **Date:** _______
