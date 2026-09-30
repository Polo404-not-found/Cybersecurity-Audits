# Finding 002: No proper authorization

**Auditor:** Juan Esteban Lopez (@Polo404-not-found)

**Category:** API1:2023 - Broken Object Level Authorization

**Severity:** Critical

**Status:** Open

**CWE:** [CWE-639](https://cwe.mitre.org/data/definitions/639.html)

---

## Description
The API stores all user data in a single global object (`User` with 
only `name` and `country`) and all financial data in another global 
object (`FinancialData`). Transactions are not associated with any user: 
there is no `user_id` field linking a transaction to its owner.

As a result, the server has no concept of data ownership. Any client—
whether authenticated or not—can read, modify, or delete any 
transaction, and can also access any other user's personal data 
(`name`, `country`).

This vulnerability is independent of Finding 001 (No Authentication). 
Even if authentication were implemented (e.g., via tokens), the absence 
of a per-user data model would still allow any authenticated user to 
access every other user's data.

---

## Impact


