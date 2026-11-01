# Engineering map

**Business software, data, integrations, algorithms and deployment**

This map connects technical areas to concrete code. Public labs are small, runnable references created in November 2026. Private examples were reviewed through manifests, domain code and selected tests; those application suites were not executed in this review.

## Public code to run

| Area | Project | Questions the implementation should answer |
| --- | --- | --- |
| Go and PostgreSQL | [Transactional outbox](https://github.com/LercZin/postgres-outbox-lab) | How do concurrent workers claim jobs? What happens after a crash or a duplicate delivery? |
| Algorithms and TypeScript | [Pathfinding lab](https://github.com/LercZin/pathfinding-lab) | Do A* and Dijkstra find the same optimal cost? How do weights and blocked cells change the search? |
| Python, SQL and data quality | [Data reconciliation](https://github.com/LercZin/data-reconciliation-lab) | How are invalid rows quarantined, imports repeated and unmatched records explained? |
| PHP and full-stack workflows | [Retail operations](https://github.com/LercZin/retail-operations-demo) | How are store isolation, stock consistency and uncertain HTTP retries handled? |
| Python API and browser integration | [UniBot](https://github.com/LercZin/unibot) | Are conversation reads/writes owner-scoped, and does the widget call the correct endpoint? |

Each repository documents its commands, verification and boundaries. Labs use fictional data and contain no commercial application source.

## Broader implementation examples

| Area | Reviewed project | Concrete implementation |
| --- | --- | --- |
| PostgreSQL | Local operations/analytics application | SQLAlchemy models and Alembic; connection validation requires PostgreSQL; account-scoped synchronization uses an advisory transaction lock and row updates. |
| Python and external APIs | Local operations/analytics application; Controlly | FastAPI services, HTTPX transports, synchronization/reconciliation and sales/route operations. |
| Local AI integration | Local operations/analytics application | Ollama requests validated against a schema; loopback-only endpoint, response limit, timeout and bounded concurrency. |
| Go | Boleto API contribution | pgx persistence, REST handlers, integer-cent values, transactional writes and FEBRABAN formatting; payment confirmation is simulated. |
| Exact financial state | SistemaFinanceiro | Brick Math decimal values, operation keys and request hashes, workspace-scoped row locks and atomic obligation settlement. |
| Identity and access | SistemaFinanceiro; retail and commerce projects | Workspace policies, revocation, version checks, second-factor verification and scoped business mutations. |
| Games and algorithms | Emberfall offline prototype | Godot/GDScript cardinal pathfinding, deterministic tie-breaking, versioned saves and compensating inventory transfers. |
| Node.js APIs and SQL | Financial backend | Express/MySQL, transaction-wrapped period closing, inventory movements and PDF/spreadsheet report generation. |
| Scheduling | Salon and clinic systems | Availability by professional/resources, conflicting room/equipment checks, transactions and optimistic version handling. |
| Frontend | Viure, Controlly and business systems | React/TypeScript, static Next.js commerce, Inertia applications, stateful forms and browser API integration. |
| Static publishing | Dal Cotone presentation site | Node-generated HTML/CSS/JS, shared catalog data, progressive interactions, local media and link/behavior checks. |

### PostgreSQL and local inference

In the operations application, `backend/app/database.py` restricts the database backend to PostgreSQL. `backend/app/store_sync.py` wraps incremental synchronization in a transaction with a PostgreSQL advisory lock. `backend/app/models.py` contains account-scoped domain records and durable publication-job models.

`backend/app/local_ai.py` implements bounded Ollama inference: local endpoint validation, explicit HTTP limits, schema-bound output and handling for insufficient evidence. This source review did not call the Meta API, load an actual local model or publish an advertisement.

The Cérebro and SelfAudit codebases also configure PostgreSQL and their database containers were observed running. Their inspected application code was at a foundation stage; planned AI tools, tenant RLS and Android collection are not treated as completed features.

### Financial consistency

SistemaFinanceiro separates identity, workspaces, accounts, ledger and planning modules. `app/Modules/Ledger/Values/Money.php` uses decimal arithmetic and explicit bounds. `PostJournal.php` checks an operation key and request hash; `SettleObligation.php` coordinates version checks and journal effects within a transaction.

`LockFinancialWorkspace.php` locks the authorization context before a financial mutation. The reviewed tests include injected failures and rollback assertions. These mechanisms are code-review evidence, not accounting certification or a claim of measured production reliability.

### Game state and recovery

The Godot prototype separates movement, combat, services, UI and persistence. `scripts/world/cardinal_pathfinder.gd` handles four-direction movement and deterministic path selection. `scripts/persistence/progression_store.gd` uses a versioned snapshot, temporary write and backup recovery path. `scripts/services/bank_service.gd` guards transaction intent and compensates a failed receiving mutation.

The inspected runtime is an offline prototype. Multiplayer authority, Nakama and PostgreSQL were future plans in that project. No online MMO or anti-cheat capability is inferred from local persistence.

## Infrastructure

Read-only inspection of two existing Linux hosts on November 1, 2026 corroborated Ubuntu/AlmaLinux environments, Docker, PostgreSQL, Redis, MySQL, Nginx, Apache/cPanel and application queue processes. This was an inventory, not a production modification or a general health certification.

Versioned deployment code in the business projects includes immutable release preparation, checksums, symlink activation, rollback, queue restart and backup metadata. Deployment permissions and recovery boundaries remain project-specific. Server addresses, account configuration, database records and credentials are excluded from this public map.

## Authorship and scope

Lucas confirms independent implementation of Pinheiro Móveis and Viure, and complete UniBot application implementation within its academic team. Original academic credits remain. The broader table identifies reviewed code and interview topics; it does not invent employers, years, team responsibilities or business results.

The existing Go checkout has commits attributed to `LercZin` while its module/origin use another account namespace. It is described as contribution evidence rather than sole authorship. The new public Go labs have their own source and verification record.

Hardware experience remains self-reported. Thermal PDF generation, TEF adapters and simulated scanner input are distinguished from physical-device validation or payment-provider homologation.

[Portfolio](../README.md) · [Executed public checks](engineering-evidence.md)
