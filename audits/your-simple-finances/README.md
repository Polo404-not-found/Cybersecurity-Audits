# Security audit: Your-Simple-Finances

**Auditor**: Juan Esteban Lopez (@Polo404-not-found)

**Date**: September 22, 2026

**Version audited**: Commit bf46973

**Status**: In progress

---
## Executive summary
`Your Simple Finances` Is a REST API built with FastAPI for personal 
finance tracking, with optional AI-powered analysis via Groq.

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
- Deployment infraestructura
- Third-party services (Groq)

---

## Methodology

The audit was performed in the following phases:
1. Reconnaissance
2. Static analysis (Code review)
3. Dynamic analysis (Local testing with curl)
4. Documentation
5. Remediation

Framework: OWASP API SECURITY TOP 10 (2023)


---


## Summary of findings

| ID  	  | Title			  	  	  | Category	| Severity     |
|---------|-----------------------------------------------|-------------|--------------|
| **001** | No authentication         	  	  	  | API2:2023   | **Critical** |
| **002** | Unrestricted resource consumption 	  	  | API4:2023	| **High**     |
| **003** | No proper authorization           	  	  | API1:2023   | **Critical** |
| **004** | Broken Object Property Level Authorization    | API3:2023   | **High**     |

