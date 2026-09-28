# Finding 001: No Authentication

**Auditor**: Juan Esteban Lopez (@Polo404-not-found)

**Category**: API2:2023 — Broken Authentication

**Severity**: Critical

**Status**: Open

**CWE**: [CWE-306](https://cwe.mitre.org/data/definitions/306.html)

---

## Description

The API does not implement any authentication mechanism. All endpoints 
accept requests without requiring credentials, tokens, or any form of 
client identification.

This means the server cannot distinguish between different clients, and 
therefore has no way to enforce authorization or data isolation. As a 
consequence, this finding directly enables Finding 002 (No Proper 
Authorization) and Finding 004 (Broken Object Property Level 
Authorization).

---

## Impact

This vulnerability breaks two pillars of the CIA triad:

- **Confidentiality**: Any unauthenticated client can read all data 
  stored by the API.
- **Integrity**: Any unauthenticated client can create, modify, or 
  delete data without restriction.

Additionally, because no identity is established:
- Actions cannot be attributed to a specific user.
- No user-level rate limiting is possible.
- Audit logs (if any) are meaningless.

---

## Proof of Concept

Two requests were made to the API without any authentication header or 
credential.

### Step 1: Retrieve all transactions without credentials

```bash
curl -X 'GET' \
  'http://127.0.0.1:8000/transactions/print' \
  -H 'accept: */*'
```

**Response (200 OK):**
```bash
  {
  "transactions": "  Type    Amount    Description\nIncome   1000.0    None"
}
```
The API returned transaction data without requiring any form of
authentication.

### Step 2: Create a transaction without credentials

```bash
curl -X 'POST' \
  'http://127.0.0.1:8000/transactions/add' \
  -H 'accept: */*' \
  -H 'Content-Type: application/json' \
  -d '{
    "type": "Income",
    "amount": 1000,
    "description": "None"
  }'
```
The API accepted the write operation without requiring any form of
authentication.

---

## Evidence

[Evidence 1](https://github.com/Polo404-not-found/Cybersecurity-Audits/tree/main/audits/your-simple-finances/Findings/Finding%20001/Evidence/Evidence1.png) - Adding a transaction without credentials.

[Evidence 2](https://github.com/Polo404-not-found/Cybersecurity-Audits/tree/main/audits/your-simple-finances/Findings/Finding%20001/Evidence/Evidence2.png) - Retrieving transactions without credentials.

[Evidence 3](https://github.com/Polo404-not-found/Cybersecurity-Audits/tree/main/audits/your-simple-finances/Findings/Finding%20001/Evidence/Evidence3.png) - Code

[Evidence 4](https://github.com/Polo404-not-found/Cybersecurity-Audits/tree/main/audits/your-simple-finances/Findings/Finding%20001/Evidence/Evidence4.png) - Code

---

## Remediation

### Short term
- **Implement token-based authentication (JWT or opaque tokens).**
- **Require a valid token on every endpoint except health checks.**
- **Return ```401 Unauthorized``` when no valid token is provided.**

### Long term
- **Use a well-tested authentication library (e.g., ```fastapi-users, python-jose, passlib```).**
- **Hash credentials with ```bcrypt``` or ```argon2```.**
- **Implement token expiration and refresh.**
- **Log authentication attempts for auditing.**
- **Add rate limiting per authenticated user.**

## References
- **OWASP API2:2023 — Broken Authentication**
- **CWE-306: Missing Authentication for Critical Function**
- **OWASP Authentication Cheat Sheet**
