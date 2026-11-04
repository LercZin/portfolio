# Technical evidence

Where to check what the portfolio says. Public repositories link to passing CI runs at a fixed commit. Private projects list the test files and the scenarios they cover, reviewed at a fixed source snapshot.

## Public: UniBot

[LercZin/unibot](https://github.com/LercZin/unibot) · academic team project; I implemented the application.

| Behavior | Inspect | How it is checked |
| --- | --- | --- |
| Intent classification, grades, domain objects and chatbot routing | [Python tests](https://github.com/LercZin/unibot/tree/main/backend/tests) | pytest, external dependencies mocked |
| Only the owner can read or write a conversation | [Authorization tests](https://github.com/LercZin/unibot/blob/main/backend/tests/test_chat_authorization.py) | HTTP routes plus the real repository SQL on in-memory SQLite |
| Chat history and academic records use different endpoints | [API tests](https://github.com/LercZin/unibot/blob/main/frontend/tests/api.test.cjs) | Node test runner |
| Token storage and expired sessions | [API tests](https://github.com/LercZin/unibot/blob/main/frontend/tests/api.test.cjs) | Persistent/session transitions and responses |
| Recovery messages never render user-supplied markup | [Recovery tests](https://github.com/LercZin/unibot/blob/main/frontend/tests/auth.test.cjs) | Lightweight browser doubles |
| The widget calls the conversation-history helper | [Widget test](https://github.com/LercZin/unibot/blob/main/frontend/tests/widget.test.cjs) | Real widget and API modules together |

```bash
python -m pytest backend/tests -q
node --test frontend/tests/*.test.cjs
```

[Passing CI run](https://github.com/LercZin/unibot/actions/runs/41310330104): **75 Python and 11 JavaScript tests** at `d63e0d3c81b9`. The authorization tests override authentication, so they do not exercise JWT validation or MySQL.

**A bug I fixed.** The frontend API client defined the same key twice, so the academic-records call silently replaced the chat-history call. I split them into `historicoChat()` and `historico()`, fixed the widget call site that still used the wrong one, and added a test that checks both URLs. In the same pass, conversation reads and inserts became owner-only: a foreign or missing conversation ID returns the same 404, and a rejected message is neither stored nor sent to the AI service. [Design notes](https://github.com/LercZin/unibot/blob/main/docs/chat-authorization.md).

## Public: Retail Operations Demo

[Repository](https://github.com/LercZin/retail-operations-demo) · [Verification](https://github.com/LercZin/retail-operations-demo/blob/main/docs/verification.md) · [Design decisions](https://github.com/LercZin/retail-operations-demo/blob/main/docs/decisions.md)

A runnable version of the retail problems from my private work, with fictional stores and accounts.

| Part | What to inspect |
| --- | --- |
| Laravel API | Store scope from the authenticated user, validation, prices in integer cents, transactional orders, idempotent replay |
| Race conditions | Separate processes competing for the last item, the same idempotency key or a conflicting payload; CI on SQLite and MySQL |
| React/TypeScript | Login, catalog, cart, order details, error states, keeping an uncertain request attempt |
| Go stock reader | Standard-library HTTP client, bounded input/output, timeouts, cancellation, read-only SKU lookup from simulated scanner input |
| Checks | 33 backend tests, 24 frontend tests, Go tests and a recorded browser walkthrough |

## Public: engineering labs

| Lab | Checks | Passing run · commit |
| --- | --- | --- |
| [data-reconciliation-lab](https://github.com/LercZin/data-reconciliation-lab) | 27 tests on Python 3.13 and 3.14, real CLI subprocesses and disposable SQLite | [CI](https://github.com/LercZin/data-reconciliation-lab/actions/runs/41315009016) · `03bc5cf6bd0d` |
| [pathfinding-lab](https://github.com/LercZin/pathfinding-lab) | 42 tests, including A*/Dijkstra agreement on 48 seeded maps | [CI](https://github.com/LercZin/pathfinding-lab/actions/runs/41315465256) · `016223aebbbf` |
| [postgres-outbox-lab](https://github.com/LercZin/postgres-outbox-lab) | 13 integration tests on a real PostgreSQL service and 9 unit tests, with the Go race detector | [CI](https://github.com/LercZin/postgres-outbox-lab/actions/runs/41315605455) · `7eb1676c1761` |

[Live pathfinding demo](https://lerczin.github.io/pathfinding-lab/).

## Private: Pinheiro Móveis

[Case study](../case-studies/pinheiro-moveis.md) · snapshot `ada4390a9478`, November 2026.

| Risk | Test file | Scenarios |
| --- | --- | --- |
| Reading another store's data | `tests/Feature/StoreScopeTest.php` | Operator sees only its store; another store's customer ID is rejected; store context is forced on creation |
| A half-applied stock count | `tests/Feature/StockCountIntegrityTest.php` | Counted and zero balances, foreign-store records ignored, rollback on failure |
| Forged or repeated payment notifications | `tests/Feature/AsaasWebhookTest.php` | Invalid token rejected, payment allocated, repeated payment processed once |

## Private: Viure Maranhão

[Case study](../case-studies/viure.md) · snapshot `77d49ba41368`, November 2026.

| Risk | Test file | Scenarios |
| --- | --- | --- |
| Repeated or early payment events | `backend/tests/Feature/AsaasWebhookTest.php` | Idempotency, invalid tokens, charge/reference matching, delayed events, stock release |
| Losing the cart when signing in | `tests/browser/guest-checkout-login.spec.ts` | Guest cart, then login, with cart/coupon/storage variations |
| Charging twice after a failed checkout | `tests/checkout-attempt.test.ts` | Stable attempt keys, account/cart separation, corrupted storage, uncertain recovery |

## Private: Cérebro

[Case study](../case-studies/cerebro.md). In active development; the case study lists what is built and what is not.

## What this page does not claim

No revenue, customer counts, benchmarks or coverage percentages. The private suites are described from their source and are not published. A detailed evaluation should tie a specific commit to its environment and test run, and I am happy to do that live in an interview.

[Portfolio](../README.md) · [Engineering map](engineering-map.md)
