# Architecture / Arquitectura

## Responsibilities

| Layer | Responsibility |
| --- | --- |
| Web card | Displays first name, stamp balance, reward and QR; periodically reads the server while visible |
| Staff/admin UI | Authenticated lookup, explicit confirmation and role-specific operations |
| Express API | Validation, Origin/CSRF, sessions, roles, business scoping and rate limits |
| Domain transaction | Visit reference, expected version, idempotency, balance and audit written atomically |
| SQLite | Durable single-host state, constraints, foreign keys and migration version |
| Operations | HTTPS proxy, encrypted consistent backups, restore verification and session revocation |

```mermaid
sequenceDiagram
  participant Staff
  participant API
  participant SQLite
  participant Customer
  Staff->>API: Read card using QR
  API-->>Staff: Balance and current version
  Staff->>Staff: Confirm visit and unique reference
  Staff->>API: Stamp + version + idempotency key
  API->>API: Revalidate access and business scope
  API->>SQLite: BEGIN IMMEDIATE
  API->>SQLite: Validate version/reference; save visit, balance and audit
  API->>SQLite: COMMIT
  API-->>Staff: Confirmed result
  Customer->>API: Refresh web card
  API-->>Customer: Updated balance
```

## Trade-offs / Decisiones

SQLite makes a modest single-instance deployment straightforward, but it requires persistent local storage and an external backup plan. Ephemeral filesystems and shared network database files are not suitable. Horizontal scaling and failover are not implemented.

The card URL is a bearer capability: possession allows reading limited card information. It is not an employee credential. Token rotation invalidates the previous card link without deleting the accumulated balance.

Duplicate protection combines different mechanisms: a ticket reference identifies the visit; a version detects concurrent state changes; an idempotency key identifies a retry. None proves that a physical service occurred. Staff accountability and operational review remain necessary.

The audit chain helps verify consistency against preserved external copies. It is not immutable against an attacker controlling both the database and all copies. The main database contains personal fields in plaintext and needs host permissions and appropriate disk protection; encrypted backups do not replace host security.

Wallet signing is a separate optional adapter. A web card remains the supported path without Apple credentials. The public demo contains no signing code, private API endpoints or live card tokens.
