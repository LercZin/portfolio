# Engineering evidence

This guide connects the portfolio's claims to specific work. It distinguishes **public code with executed tests**, **reviewed private implementation evidence** and **additional self-reported experience**.

## Public code: UniBot

Repository: [LercZin/unibot](https://github.com/LercZin/unibot). Lucas confirms responsibility for the complete application implementation. The academic team credits remain preserved; responsibilities of the other credited members are not inferred.

| Behavior | Inspect | Verification |
| --- | --- | --- |
| Intent classification, grades, domain objects and chatbot routing | [Python tests](https://github.com/LercZin/unibot/tree/main/backend/tests) | Included in 75 passing Python tests; dependencies mocked where required |
| Conversation ownership on reads and writes | [Authorization tests](https://github.com/LercZin/unibot/blob/main/backend/tests/test_chat_authorization.py) | HTTP routes and actual repository SQL checked using an in-memory SQLite adapter |
| Distinct chat history and academic-record endpoints | [API regression tests](https://github.com/LercZin/unibot/blob/main/frontend/tests/api.test.cjs) | Executed with Node's test runner |
| Token-storage transitions and expired-session handling | [API regression tests](https://github.com/LercZin/unibot/blob/main/frontend/tests/api.test.cjs) | Persistent/session transitions and response behavior checked |
| Recovery messaging and user-markup handling | [Recovery regression tests](https://github.com/LercZin/unibot/blob/main/frontend/tests/auth.test.cjs) | Checked with lightweight browser doubles |
| Widget calls the conversation-history helper | [Widget integration test](https://github.com/LercZin/unibot/blob/main/frontend/tests/widget.test.cjs) | Actual widget and API modules combined with lightweight browser doubles |

Run from the repository root after installing the documented Python dependencies:

```bash
python -m pytest backend/tests -q
node --test frontend/tests/*.test.cjs
```

The [passing CI run](https://github.com/LercZin/unibot/actions/runs/41310330104) executed **75 Python tests and 11 JavaScript tests** at commit `d63e0d3c81b9344a31cc4545df6ac3151d365c68`. Authentication is overridden for the authorization tests; this does not certify JWT validation, MySQL behavior, a full browser journey or the live AI provider.

### Example of a regression addressed

The frontend originally used the same object key for chat history and academic records, so the later definition replaced the earlier one. The client now exposes `historicoChat()` for conversations while preserving `historico()` for academic records. A regression test verifies both URLs.

Recovery messaging now states that the academic prototype does not send reset emails, and avoids putting user-supplied markup into HTML. The tests exercise both behaviors.

Conversation access now requires the authenticated owner on both reads and inserts. Foreign and missing conversation IDs return the same 404 response. Rejected message submissions neither persist content nor invoke the AI service. A regression test also exercises the widget call site, which previously still selected the academic-record helper after the API methods had been separated. [Design and regression scope](https://github.com/LercZin/unibot/blob/main/docs/chat-authorization.md).

## Private implementation review: Pinheiro Móveis

[Case study](../case-studies/pinheiro-moveis.md). Source snapshot reviewed: `ada4390a947819370b60fc5bd5ea21bcaf1fa0e1`, November 1, 2026.

| Risk | Reviewed test file | Scenarios present |
| --- | --- | --- |
| Cross-store data access | `tests/Feature/StoreScopeTest.php` | Operator sees its store; another store's customer ID is rejected; store context is enforced on creation |
| Inconsistent inventory reconciliation | `tests/Feature/StockCountIntegrityTest.php` | Counted balances, zero balances, ignored foreign-store records and rollback on failure |
| Duplicate or unauthenticated payment notifications | `tests/Feature/AsaasWebhookTest.php` | Invalid-token rejection, payment allocation and repeated-payment processing |

The source and assertions were inspected for this documentation, including `StockService::count()` and its transaction, row lock and expected-count checks. The private application test suite was **not executed during this portfolio pass**. No production records or private source are included here.

## Private implementation review: Viure Maranhão

[Case study](../case-studies/viure.md). Source snapshot reviewed: `77d49ba413684f25c375de4f76a66d5d425104b8`, November 1, 2026.

| Risk | Reviewed test file | Scenarios present |
| --- | --- | --- |
| Repeated payment events and incorrect order matching | `backend/tests/Feature/AsaasWebhookTest.php` | Idempotency, invalid tokens, charge/reference matching, delayed events and stock release |
| Losing cart context during authentication | `tests/browser/guest-checkout-login.spec.ts` | Guest shopping followed by login with cart/coupon/storage variations |
| Duplicate charges during checkout recovery | `tests/checkout-attempt.test.ts` | Stable attempt keys, account/cart separation, corrupted storage and uncertain recovery failures |

The review also inspected the webhook service's pending-event replay and transaction boundaries, and the checkout attempt record's account/cart scoping and storage-failure handling. The [case study](../case-studies/viure.md#technical-decisions) describes the tradeoffs, including the absence of a durable email outbox. These are reviewed test scenarios, not a claim that the private suites passed in this review or that every feature is enabled in the current deployment.

## Public PHP, React and Go: Retail Operations Demo

[Repository](https://github.com/LercZin/retail-operations-demo) · [Verification record](https://github.com/LercZin/retail-operations-demo/blob/main/docs/verification.md) · [Design decisions](https://github.com/LercZin/retail-operations-demo/blob/main/docs/decisions.md).

Created in November 2026 as a runnable portfolio reference with fictional stores and accounts. It makes the main stack directly reviewable without disclosing commercial source or customer records.

| Evidence | What to inspect |
| --- | --- |
| PHP/Laravel API | Authenticated store scope, validation, integer-cent prices, transactional order creation and idempotent replay |
| Database races | Separate request processes competing for the last item, the same key or a conflicting payload; SQLite and MySQL CI jobs |
| React/TypeScript | Login, store catalog, cart, order details, failure states and preservation of uncertain request attempts |
| Go adapter | Standard-library HTTP client, bounded input/output, timeouts, cancellation and read-only SKU lookup |
| Verification | 33 backend tests with SQLite locally, 24 frontend tests, Go tests and an actual Edge/API walkthrough; CI identifies its database-specific results |

The reader accepts simulated keyboard-style scans through stdin. No physical scanner, serial connection, device driver or hardware model was validated.

## Additional public engineering labs

Created in November 2026 with fictional data. These runs verify the linked source revisions.

| Project | Executed checks | Passing run and revision |
| --- | --- | --- |
| [data-reconciliation-lab](https://github.com/LercZin/data-reconciliation-lab) | 27 tests on Python 3.13 and 3.14 | [CI](https://github.com/LercZin/data-reconciliation-lab/actions/runs/41315009016) · `03bc5cf6bd0d` |
| [pathfinding-lab](https://github.com/LercZin/pathfinding-lab) | 42 tests, including A*/Dijkstra agreement on 48 seeded maps | [CI](https://github.com/LercZin/pathfinding-lab/actions/runs/41315465256) · `016223aebbbf` |
| [postgres-outbox-lab](https://github.com/LercZin/postgres-outbox-lab) | 13 PostgreSQL integration tests and 9 unit tests | [CI](https://github.com/LercZin/postgres-outbox-lab/actions/runs/41315605455) · `7eb1676c1761` |

The outbox run uses an actual PostgreSQL service and the Go race detector. The reconciliation matrix runs both Python versions with disposable SQLite databases. Pathfinding includes a separately recorded local Edge walkthrough; its unit suite does not automate browser accessibility. [Live pathfinding demonstration](https://lerczin.github.io/pathfinding-lab/) · [Broader engineering map](engineering-map.md).

## Additional experience

Lucas has reported broader hardware/software integration experience. The new Go reader provides inspectable Go code and automated checks; historical hardware deployments still lack a detailed verified case study and are not asserted here.

## Scope

This portfolio is a technical introduction, not an independent security audit. It does not assert revenue, customer numbers, performance benchmarks or coverage percentages. Detailed evaluation should connect a specific source revision to the relevant environment and test run.
