# Security audit: Your-Simple-Finances

**Auditor**: Juan Esteban Lopez (@Polo404-not-found)

**Date**: September 22, 2026

**Version audited**: Commit bf46973

**Status**: In progress

---
## Executive summary
`Your-Simple-Finances` is a REST API built with FastAPI for personal finance tracking,
with optional AI-powered analysis via Groq.

This audit reviews the API's security posture, focusing on 
authentication, authorization, data isolation and integration with
external services. The audit was performed using the OWASP API Security
Top 10 (2023) as the primary methodology.

The audit identified **4 vulnerabilities**: 2 Critical, 2 High.

## Scope 

### In scope
- Source code of the API (FastAPI endpoints)
- Data models (`financial_Info.py`, `user_Info.py`)
- AI integration (`AI_Client.py`)

### Out of scope
- Frontend / UI
- Deployment infrastructure
- Third-party services (Groq)

---

## Methodology

The audit was performed in the following phases:
1. Reconnaissance
2. Static analysis (Code review)
3. Dynamic analysis (Local testing with curl)
4. Documentation
5. Remediation

Framework: OWASP API Security Top 10 (2023)


---


## Summary of Findings

| **ID**  | **Title**			  	  	  | **Category**  | **Severity** |
|---------|-----------------------------------------------|---------------|--------------|
| **001** | No authentication         	  	  	  | API2:2023     | **Critical** |
| **002** | No proper authorization           	  	  | API1:2023     | **Critical** |
| **003** | Unrestricted resource consumption 	  	  | API4:2023	  | **High**     |
| **004** | Broken Object Property Level Authorization    | API3:2023     | **High**     |

---

## Finding
- [Finding 001: No authentication](https://github.com/Polo404-not-found/Cybersecurity-Audits/edit/main/audits/your-simple-finances/Findings/Finding%20001)
---

## Severity Definitions

|    **Severity**   | **Description** 									|			
|-------------------|-----------------------------------------------------------------------------------|
|    **Critical**   | Exploitable remotely without authentication, leads to full compromise of CIA      |
|    **High**       | Exploitable with low complexity, significant impact on one or more pillars of CIA |
|    **Medium**     | Exploitable under specific conditions, or limited impact                          |
|    **Low**        | Requires privileged access or has minimal impact 					|
| **Informational** | Best practice recommendation, no direct security impact 				|

---

## Coverage Limitations

This audit focused on authentication, authorization, and resource 
consumption in the API layer. The following areas were not reviewed:

- Frontend security
- Network configuration
- Dependency vulnerabilities (SCA)
- Cryptographic implementations
- Deployment environment

Due to the time-boxed nature of this audit, findings should not be 
considered exhaustive.

---

## Disclaimer

This audit was performed for educational purposes on a personal project. 
It is not a comprehensive security assessment. The absence of findings 
in a specific area does not guarantee the absence of vulnerabilities. 
The author is not responsible for any misuse of the information presented 
here.

---

## References

- [OWASP API Security Top 10 (2023)](https://owasp.org/API-Security/editions/2023/en/0x11-t10/)
- [OWASP Top 10 (2021)](https://owasp.org/Top10/)
- [CWE - Common Weakness Enumeration](https://cwe.mitre.org/)
- [your-simple-finances repository](https://github.com/Polo404-not-found/your-simple-finances)

