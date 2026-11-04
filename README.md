# Lucas Emanuel Ramos Carvalho

**Full-stack developer · business systems, integrations and data**  
PHP / Laravel · Python / FastAPI · Go · React / TypeScript · PostgreSQL · MySQL · Linux

I build the software behind a business: **selling, managing stock, collecting payments and following through on deliveries**. I take a feature from the conversation with the people who use it to the screen, the backend rules, the database, the external integrations and the deployment.

[Website](https://lerczin.github.io/portfolio/) · [Email](mailto:lercsz1472@gmail.com) · [GitHub](https://github.com/LercZin) · [Español](README.es.md)

## Projects

| Project | What it does | My role · stack | Look at |
| --- | --- | --- | --- |
| **Pinheiro Móveis** | Runs a multi-store furniture retailer: sales, stock, installment payments, delivery and assembly. | Sole developer since 2022 · Laravel, Livewire, MySQL | [Case study](case-studies/pinheiro-moveis.md) · [Site](https://pinheiromoveis.com) |
| **Viure Maranhão** | Online store with sign-in, checkout, payment webhooks and shipping quotes. | Sole developer since 2022 · Next.js, React, TypeScript, Laravel, MySQL | [Case study](case-studies/viure.md) · [Site](https://viuremaranhao.com.br) |
| **Cérebro** | Multi-tenant platform for AI-assisted customer service over WhatsApp, isolated per company in PostgreSQL. | Lead developer, in progress · FastAPI, PostgreSQL RLS, Redis, React | [Case study](case-studies/cerebro.md) |
| **Retail Operations Demo** | Public, runnable version of the retail problems above, with fictional stores. | Sole developer · Laravel, React/TypeScript, Go, MySQL/SQLite | [Code](https://github.com/LercZin/retail-operations-demo) · [Verification](https://github.com/LercZin/retail-operations-demo/blob/main/docs/verification.md) |
| **UniBot** | Student portal assistant: academic data, FAQs and an optional AI chat. | Implemented the application in an academic team · FastAPI, MySQL, JavaScript | [Code](https://github.com/LercZin/unibot) · [CI](https://github.com/LercZin/unibot/actions/workflows/tests.yml) |

The commercial projects are private, so their case studies explain the decisions and the tests instead of showing the code. The public repositories can be cloned and run.

## Engineering labs

Small projects with fictional data, each focused on one problem that is easy to get wrong.

| Lab | Problem | Look at |
| --- | --- | --- |
| **PostgreSQL Outbox** | Go/pgx workers claiming jobs concurrently, surviving crashes and duplicate deliveries. | [Code](https://github.com/LercZin/postgres-outbox-lab) |
| **Pathfinding** | A* and Dijkstra on weighted grids in TypeScript, with step-by-step playback. | [Code](https://github.com/LercZin/pathfinding-lab) · [Demo](https://lerczin.github.io/pathfinding-lab/) |
| **Data Reconciliation** | Python/SQL import that quarantines bad rows, can be re-run safely and explains every mismatch. | [Code](https://github.com/LercZin/data-reconciliation-lab) |

## Problems I have solved

- **A store must never see another store's data.** In Pinheiro Móveis the store scope is enforced on the server, on every query and on record creation, with tests that try to read another store's customer by ID.
- **A stock count that fails halfway must not leave half the stock adjusted.** The whole count runs in one transaction with row locks, and it is rejected if the balance changed since the operator started counting.
- **Payment providers send the same notification more than once, sometimes before the charge exists.** Viure stores each event, applies it once and replays it when the matching payment appears.
- **A retried checkout must not charge the customer twice.** The checkout keeps one attempt key through network failures and stops instead of silently creating a new charge.
- **Tenant isolation should not depend on remembering a `WHERE` clause.** Cérebro uses PostgreSQL row-level security, so the database itself refuses rows from another company.

## Skills

| Area | Technologies | Where |
| --- | --- | --- |
| Backend | PHP/Laravel, Python/FastAPI, Go, REST APIs, authorization, transactions | All projects |
| Data | PostgreSQL (RLS, advisory locks), MySQL, SQLite, SQLAlchemy, Alembic, pgx | Cérebro, labs, business systems |
| Frontend | React, TypeScript, Next.js, Livewire, Tailwind, HTML/CSS | Viure, Cérebro, Pinheiro Móveis |
| Integrations | Asaas payments, Melhor Envio shipping, WhatsApp Cloud API, webhooks | Viure, Pinheiro Móveis, Cérebro |
| Testing | PHPUnit, pytest, Playwright, JavaScript test runners, GitHub Actions CI | All public repositories |
| Delivery | Git, Docker Compose, Linux VPS, Nginx/Apache, queues, release and rollback scripts | Business systems, Cérebro |

More detail: [engineering map](docs/engineering-map.md) (areas → code) and [technical evidence](docs/engineering-evidence.md) (tests and CI runs).

## About me

Before and alongside software, I worked in **furniture assembly, in-store and online sales, and marketing**. That is why my projects start from how the work is actually done: I have done the selling and the assembly myself.

I started Pinheiro Móveis and Viure in **2022**, while studying Computer Science at **Anhanguera**, graduating in **December 2026**.

- **Languages:** Portuguese (native), English and Spanish (intermediate).
- **Location:** São Luís, Brazil. Open to relocating to Spain (I need the employer to sponsor the work permit). Available for remote interviews and technical tests.
- **Looking for:** software developer, full-stack, Laravel backend or React/TypeScript roles.

**Contact:** [lercsz1472@gmail.com](mailto:lercsz1472@gmail.com) · machine-readable profile: [`candidate.json`](candidate.json)

<sub>No revenue, traffic or performance figures appear here because I have not measured them. Private test suites are described from their source; public repositories link to their passing CI runs.</sub>
