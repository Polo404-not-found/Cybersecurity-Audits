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

This finding breaks all three pillars of the CIA triad:

- **Confidentiality**: Any client can read the personal data 
  (`name`, `country`) and financial transactions of every other user. 
  The absence of a `user_id` association means there is no concept of 
  data ownership, so every record is visible to everyone.

- **Integrity**: Any client can modify or delete transactions that 
  belong to another user. Since the server cannot determine who owns 
  each record, it cannot reject unauthorized modifications.

- **Availability**: A malicious client could flood the global 
  `FinancialData` object with fake transactions or delete existing 
  ones, corrupting the shared state and making the service unusable 
  for legitimate users.

Additionally, this finding exposes **Personally Identifiable 
Information (PII)**. Even though the `User` model only contains 
`name` and `country`, both are considered personal data under most 
data protection regulations (e.g., GDPR). Unauthorized access to this 
data constitutes a privacy breach.

Finally, this vulnerability is independent of Finding 001 (No 
Authentication). Even if authentication were implemented, the absence 
of a per-user data model would still allow any authenticated user to 
access every other user's data.

--- 

## Proof of Concept

Two independent HTTP clients were used to simulate two distinct users. 
No authentication was provided in either case, which reflects the 
current state of the API.

### Step 1: Client A creates a transaction

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

The transaction is stored in the global `FinancialData` object. Since
transactions have no `user_id` field,  the record is not associated with 
any specific user.

### Step 2: Client B retrieves all transactions

```bash
curl -X 'GET' \
  'http://127.0.0.1:8000/transactions/print' \
  -H 'accept: */*'
```
Response (200 OK):

```bash
{
  "transactions": "  Type    Amount    Description\nIncome   1000.0    None"
}
```
Client B retrieved the transaction created by the client A. The server did 
not distinguish between de two clients, because:

- There is no authentication method (Finding 001).
- There is no `user_id` associated with transactions.
- All requests read from and write to the same global state.

This demostrates that the API does not enforece object-level 
authorization: any client can acces any other client's data.

### Step 3: Client B modifies the shared state

```bash
curl -X 'POST' \
  'http://127.0.0.1:8000/transactions/add' \
  -H 'accept: */*' \
  -H 'Content-Type: application/json' \
  -d '{
    "type": "Expense",
    "amount": 5000,
    "description": "Injected by Client B"
  }'
```

Client B was able to add a transaction to the same global state that 
Client A is using. There is no mechanism to prevent one client from 
modifying data that conceptually belongs to other client.

---

## Evidences

---

## Remediation 

### Short term
### Medium term
### Long term 

---

## References
