# Cérebro

**Multi-tenant platform for AI-assisted customer service · Lead developer · Started November 2026 · In development**

[Portfolio](../README.md)

## Business problem

Small companies answer customers on WhatsApp all day: balances, due dates, order status. The goal is an assistant that answers from the company's own data and hands the conversation to a person whenever it doesn't have the answer. It must never make up a price, date or condition.

The platform serves several companies from different business sectors. So the first job was not the AI. It was making sure one company's data can never reach another company, and that everything coming in from the outside (webhooks, spreadsheets, sign-ins) is treated as hostile.

## What is built

| Part | What it does |
| --- | --- |
| Tenancy | Each company's rows are isolated by PostgreSQL row-level security. The company is resolved only on the server, never from the request. |
| Panel access | Sign-in, TOTP second factor with recovery codes, invites, password reset, re-authentication for sensitive actions, per-role permission matrix. |
| WhatsApp intake | Meta Cloud API webhook: signature check, deduplication, media download, account-health tracking. |
| Spreadsheet import | CSV/XLSX upload processed in a worker, with row quarantine and a safe export. |
| Operations | Docker Compose with secrets, task queue with retries and dead-lettering, deploy with backup and automatic rollback. |

**Stack:** Python, FastAPI, SQLAlchemy 2.0 (async), Alembic, PostgreSQL, Taskiq + Redis, React + TypeScript + Vite, Docker Compose.

## Technical decisions

### Isolation enforced by the database

Every tenant table has `ENABLE` and `FORCE ROW LEVEL SECURITY` plus a restrictive guard policy. The current company is set with a transaction-local `set_config` as the first statement of every transaction. With no company set, queries return nothing instead of leaking rows through a reused pooled connection.

The application role is `NOBYPASSRLS`. Only one role without login can create companies, and the lookups that must cross tenants (resolving a session, or a WhatsApp number to its company) run through narrow `SECURITY DEFINER` functions.

A catalog test fails the build if any tenant table loses RLS or its guard policy. It also fails if a view lacks `security_invoker` or a function is executable by `PUBLIC`. The tests run against real PostgreSQL in containers, using the production role scripts.

### Sign-in built to current guidance

- Passwords use Argon2id and follow NIST 800-63B: length over composition rules, a blocklist, and a breached-password check through k-anonymity.
- Repeated failures get exponentially longer waits, and an unknown account takes as long to answer as a wrong password.
- Sessions live on the server. Only a hash of the token is stored, with idle and absolute timeouts and a `__Host-` cookie set to `SameSite=Strict`.
- The TOTP secret is encrypted with AES-256-GCM, bound to its row, and a replayed code is rejected atomically in the database.
- Permissions are deny-by-default: the application refuses to start if a route has no entry in the permission matrix.

### Webhooks are untrusted and repeatable

The `X-Hub-Signature-256` HMAC is checked in constant time over the raw bytes, before any parsing. Each event gets a deduplication key under a unique constraint, so redeliveries are harmless. A periodic sweep re-queues anything that stalled. Media is downloaded right away, because Meta's URLs expire, and checked by size, magic bytes and hash before it is stored.

### Spreadsheets are hostile input

XLSX files are parsed with external links, macros and XML entity expansion disabled. Row, column, size and time limits reject oversized files outright. Every cell is normalized and stripped of control and zero-width characters. Rows that look like prompt injection are quarantined with a reason. Quarantined rows never reach the model. Exports are protected against CSV formula injection.

### Logs that cannot leak

The structured JSON logs carry a request ID and a company ID. Messages pass through a redactor for tokens, emails, phone numbers, card numbers, Brazilian tax IDs (CPF and CNPJ, including the new alphanumeric CNPJ, checked by check digit) and Pix payment codes (validated by CRC). The audit log and the usage ledger are append-only at the permission level: the app role can insert but not update or delete.

## Verification

| Gate | Detail |
| --- | --- |
| Tests | 841 backend tests (pytest, real PostgreSQL and Redis), about 230 panel tests (Vitest) |
| Coverage | Branch coverage, build fails below 90% |
| Static checks | Ruff (including security rules), Pyright strict, import-linter layers, Biome, TypeScript |
| Security scans | Semgrep, gitleaks over the full history, pip-audit, npm audit, Trivy image scan, zizmor and actionlint for workflows |
| Migrations | 40 Alembic migrations, linted with Squawk and checked for lock timeouts |
| Code size | No file over 100 lines, checked in CI |

Decisions are recorded in 8 architecture decision records (stack, code organization, testing and security, usage cost accounting, rules as data across business sectors).

## Not built yet

- **The AI agent itself.** The design and rules are written, but there is no model integration yet. The foundation came first on purpose.
- **Sending WhatsApp messages.** The platform receives and processes messages; replies and templates come later.
- **The business data model** (customers, invoices, payments). It waits on interviews with the pilot company.
- A daily encrypted off-server backup. Today there is a backup before every migration.

## Topics I can explain in an interview

- Why row-level security, and how to test that it stays on.
- Resolving the tenant from the session or the WhatsApp number without trusting the request.
- Making webhook processing idempotent under concurrent workers.
- Treating spreadsheets and messages as untrusted input before they reach an AI model.
- Keeping a large codebase consistent with automated rules and CI gates.

**Contact:** [lercsz1472@gmail.com](mailto:lercsz1472@gmail.com)
