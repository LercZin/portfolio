# Engineering map

**Business software, data, integrations, algorithms and deployment**

This page connects each technical area to code I can walk you through. The public repositories run on your machine; the private ones are described by file and behavior, without publishing their source.

## Public code to run

| Area | Project | Questions the code answers |
| --- | --- | --- |
| Go and PostgreSQL | [Transactional outbox](https://github.com/LercZin/postgres-outbox-lab) | How do concurrent workers claim jobs? What happens after a crash or a duplicate delivery? |
| Algorithms and TypeScript | [Pathfinding lab](https://github.com/LercZin/pathfinding-lab) | Do A* and Dijkstra find the same optimal cost? How do weights and blocked cells change the search? |
| Python, SQL and data quality | [Data reconciliation](https://github.com/LercZin/data-reconciliation-lab) | How are invalid rows quarantined, imports repeated and unmatched records explained? |
| PHP and full-stack workflows | [Retail operations](https://github.com/LercZin/retail-operations-demo) | How are store isolation, stock consistency and uncertain HTTP retries handled? |
| Python API and browser integration | [UniBot](https://github.com/LercZin/unibot) | Are conversation reads and writes owner-scoped, and does the widget call the right endpoint? |

Each repository documents how to run it, what its tests cover and what it does not do.

## Private projects

| Area | Project | What I implemented |
| --- | --- | --- |
| Multi-tenant SaaS | [Cérebro](../case-studies/cerebro.md) | FastAPI, PostgreSQL row-level security per company, panel sign-in with MFA, WhatsApp Cloud API webhook, spreadsheet import with quarantine. |
| PostgreSQL | Operations and analytics application | SQLAlchemy and Alembic on PostgreSQL only; account-scoped synchronization under an advisory transaction lock. |
| Python and external APIs | Operations application; Controlly | FastAPI services, HTTPX clients, synchronization, reconciliation and sales/route operations. |
| Local AI | Operations application | Ollama calls validated against a schema, loopback-only endpoint, response size limit, timeout and bounded concurrency. |
| Exact money | SistemaFinanceiro | Decimal values, operation keys with request hashes, workspace row locks and atomic settlement of obligations. |
| Identity and access | SistemaFinanceiro; retail and commerce projects | Workspace policies, session revocation, version checks, second factor and scoped business changes. |
| Go | Boleto API (contribution) | pgx persistence, REST handlers, integer cents, transactional writes and FEBRABAN formatting; payment confirmation simulated. |
| Node.js and SQL | Financial backend | Express/MySQL, period closing inside a transaction, inventory movements, PDF and spreadsheet reports. |
| Scheduling | Salon and clinic systems | Availability per professional and resource, room/equipment conflict checks, optimistic versioning. |
| Games | Emberfall (offline prototype) | Godot/GDScript four-direction pathfinding with deterministic tie-breaking, versioned saves, compensating inventory transfers. |
| Frontend | Viure, Controlly, business systems | React/TypeScript, static Next.js commerce, Inertia applications, stateful forms. |
| Static publishing | Dal Cotone presentation site | Node-generated HTML/CSS/JS from shared catalog data, progressive interactions and link checks. |

### PostgreSQL and local inference

In the operations application, `backend/app/database.py` refuses any database other than PostgreSQL. `backend/app/store_sync.py` runs incremental synchronization in one transaction under a PostgreSQL advisory lock, so two syncs for the same account cannot interleave. `backend/app/local_ai.py` calls a local Ollama model with explicit HTTP limits, validates the answer against a schema and returns "not enough evidence" instead of guessing.

### Financial consistency

SistemaFinanceiro separates identity, workspaces, accounts, ledger and planning modules. `Money.php` uses decimal arithmetic with explicit bounds. `PostJournal.php` checks an operation key and a request hash, so a retry is not posted twice and a key reused with a different payload is detected. `SettleObligation.php` checks versions and writes the journal entries in one transaction, and `LockFinancialWorkspace.php` locks the workspace before any financial change. The tests inject failures and assert the rollback.

### Game state and recovery

The Godot prototype separates movement, combat, services, UI and persistence. `cardinal_pathfinder.gd` picks paths deterministically. `progression_store.gd` writes a versioned snapshot to a temporary file and keeps a backup to recover from. `bank_service.gd` compensates when the receiving side of a transfer fails. Multiplayer was planned, not built.

## Infrastructure

I operate Linux servers (Ubuntu and AlmaLinux) running Docker, PostgreSQL, Redis, MySQL, Nginx, Apache/cPanel and application queue workers.

The deployment scripts in the business projects prepare immutable releases, verify checksums, switch the active release with a symlink, restart queues, record backup metadata and roll back to the previous release. Server addresses, accounts and credentials stay out of this repository.

## Authorship

I developed Pinheiro Móveis, Viure and the public labs on my own, and implemented the UniBot application within an academic team whose credits are preserved in that repository. The Boleto API is listed as a contribution. I also have hands-on hardware/software integration experience; the inspectable part is the Go stock reader in the retail demo, which reads simulated scanner input.

[Portfolio](../README.md) · [Technical evidence](engineering-evidence.md)
