# Lucas Emanuel Ramos Carvalho · Software Portfolio

**Full-stack applications, business systems and integrations**  
PHP / Laravel · React / TypeScript · Next.js · MySQL · REST APIs

I develop software for the work behind a business: **selling, managing inventory, collecting payments and following through on deliveries**. My contribution covers requirements, interfaces, backend rules, databases, external integrations and deployment.

[Contact](mailto:lercsz1472@gmail.com) · [GitHub](https://github.com/LercZin) · [Español](README.es.md) · [Technical evidence](docs/engineering-evidence.md) · [Structured profile](candidate.json)

## Start with these projects

| Project | Problem solved | Contribution and technologies | Review |
| --- | --- | --- | --- |
| **Retail Operations Demo** | Demonstrate isolated stores, stock deductions and safe order retries with fictional data. | Laravel, React/TypeScript, SQLite/MySQL and Go · created November 2026 for public review. | [Run and inspect](https://github.com/LercZin/retail-operations-demo) · [Verification](https://github.com/LercZin/retail-operations-demo/blob/main/docs/verification.md) |
| **Pinheiro Móveis** | Connect store-level sales, stock, installment payments and delivery operations. | Independent development · Laravel, Livewire, MySQL. | [Case study](case-studies/pinheiro-moveis.md) |
| **Viure Maranhão** | Connect a public commerce interface with authentication, payment handling and shipping services. | Independent development · Next.js, React, TypeScript, Laravel, MySQL. | [Case study](case-studies/viure.md) · [Website](https://viuremaranhao.com.br) |
| **UniBot** | Bring academic information, FAQs and an optional AI assistant into a student portal. | Application implementation · academic team · Python, FastAPI, MySQL, JavaScript. | [Public source](https://github.com/LercZin/unibot) · [CI](https://github.com/LercZin/unibot/actions/workflows/tests.yml) |

## Engineering examples

**Store isolation and inventory integrity.** Pinheiro Móveis includes tests for cross-store access, forced store context on creation, inventory reconciliation and rollback when an operation fails.

**Repeatable payment notifications and checkout recovery.** Viure includes tests for webhook idempotency, payment/order matching, failed-payment stock release and recovery of a checkout attempt without silently creating a new charge.

**Runnable PHP and React evidence.** The [Retail Operations Demo](https://github.com/LercZin/retail-operations-demo) exposes the complete API, interface, stock reader, tests and CI. Its design notes discuss transaction scope, duplicate requests and database concurrency.

**Public code with regression checks.** UniBot's CI runs **75 Python tests and 11 JavaScript tests**. The suites cover conversation ownership through HTTP routes and repository SQL, widget/history routing, token persistence, expired sessions and recovery messaging. [Inspect the evidence and its scope](docs/engineering-evidence.md).

## Skills connected to work

| Area | Technologies and practice | Evidence |
| --- | --- | --- |
| Backend | PHP, Laravel, REST APIs, MySQL, authorization and transactions | Pinheiro Móveis; Viure |
| Frontend | React, TypeScript, Next.js, Livewire, HTML/CSS, API integration | Viure; Pinheiro Móveis |
| Integrations | Asaas payments, Melhor Envio shipping, webhooks | Viure |
| Testing | PHPUnit, Playwright, pytest, JavaScript regression tests | Project case studies; public UniBot source and CI |
| Delivery | Git, Linux VPS environments and deployment workflows | Commercial project documentation |
| Go | Standard-library HTTP client, context/timeouts, stdin and NDJSON | [Public stock-reader implementation and tests](https://github.com/LercZin/retail-operations-demo/tree/main/integrations/stock-reader) |
| Additional experience | Hardware/software integration | Experience reported by Lucas; the public reader uses simulated input and does not validate a physical device |

## Professional context

I have practical experience in **furniture assembly, in-person and online sales, and marketing management**. This informs how I understand operational workflows and user needs.

I started the independent Pinheiro Móveis and Viure projects in **2022**, alongside my Computer Science studies at **Anhanguera**. Graduation is expected in **December 2026**.

**Languages:** Portuguese native; English and Spanish intermediate.  
**Location:** São Luís, Brazil. Open to relocation to Spain with the required employer-supported work authorization. Available for remote interviews and technical assessments.

## Project access and verification

Commercial source code and operational records remain private. The case studies describe implementation decisions and reviewed test cases; they do not claim that the private suites were executed in this portfolio review. UniBot is a public academic prototype: Lucas confirms complete application implementation, and the academic team credits remain preserved. The retail demo is a new portfolio reference, not an earlier customer deployment.

The structured [`candidate.json`](candidate.json) contains the same professional facts and project links in a machine-readable format. It includes no hidden instructions or hiring scores. No revenue, customer-volume or performance metrics are asserted without measurements.

**Contact:** [lercsz1472@gmail.com](mailto:lercsz1472@gmail.com)
