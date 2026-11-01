# Engineering evidence

This guide connects the portfolio's claims to specific work. It distinguishes **public code with executed tests**, **reviewed private implementation evidence** and **additional self-reported experience**.

## Public code: UniBot

Repository: [LercZin/unibot](https://github.com/LercZin/unibot). Collaborative academic project with contributor credits preserved.

| Behavior | Inspect | Verification |
| --- | --- | --- |
| Intent classification, grades, domain objects and chatbot routing | [Python tests](https://github.com/LercZin/unibot/tree/main/backend/tests) | 59 tests executed locally; dependencies mocked where required |
| Distinct chat history and academic-record endpoints | [API regression tests](https://github.com/LercZin/unibot/blob/main/frontend/tests/api.test.cjs) | Executed with Node's test runner |
| Token-storage transitions and expired-session handling | [API regression tests](https://github.com/LercZin/unibot/blob/main/frontend/tests/api.test.cjs) | Persistent/session transitions and response behavior checked |
| Recovery messaging and user-markup handling | [Recovery regression tests](https://github.com/LercZin/unibot/blob/main/frontend/tests/auth.test.cjs) | Checked with lightweight browser doubles |

Run from the repository root after installing the documented Python dependencies:

```bash
python -m pytest backend/tests -q
node --test frontend/tests/*.test.cjs
```

The [CI workflow](https://github.com/LercZin/unibot/actions/workflows/tests.yml) runs both suites. These tests do not make live AI requests or establish database/browser integration coverage.

### Example of a regression addressed

The frontend originally used the same object key for chat history and academic records, so the later definition replaced the earlier one. The client now exposes `historicoChat()` for conversations while preserving `historico()` for academic records. A regression test verifies both URLs.

Recovery messaging now states that the academic prototype does not send reset emails, and avoids putting user-supplied markup into HTML. The tests exercise both behaviors.

## Private implementation review: Pinheiro Móveis

[Case study](../case-studies/pinheiro-moveis.md). Source snapshot reviewed: `ada4390a947819370b60fc5bd5ea21bcaf1fa0e1`, November 1, 2026.

| Risk | Reviewed test file | Scenarios present |
| --- | --- | --- |
| Cross-store data access | `tests/Feature/StoreScopeTest.php` | Operator sees its store; another store's customer ID is rejected; store context is enforced on creation |
| Inconsistent inventory reconciliation | `tests/Feature/StockCountIntegrityTest.php` | Counted balances, zero balances, ignored foreign-store records and rollback on failure |
| Duplicate or unauthenticated payment notifications | `tests/Feature/AsaasWebhookTest.php` | Invalid-token rejection, payment allocation and repeated-payment processing |

The source and assertions were inspected for this documentation. The private application test suite was **not executed during this portfolio pass**. No production records or private source are included here.

## Private implementation review: Viure Maranhão

[Case study](../case-studies/viure.md). Source snapshot reviewed: `77d49ba413684f25c375de4f76a66d5d425104b8`, November 1, 2026.

| Risk | Reviewed test file | Scenarios present |
| --- | --- | --- |
| Repeated payment events and incorrect order matching | `backend/tests/Feature/AsaasWebhookTest.php` | Idempotency, invalid tokens, charge/reference matching, delayed events and stock release |
| Losing cart context during authentication | `tests/browser/guest-checkout-login.spec.ts` | Guest shopping followed by login with cart/coupon/storage variations |
| Duplicate charges during checkout recovery | `tests/checkout-attempt.test.ts` | Stable attempt keys, account/cart separation, corrupted storage and uncertain recovery failures |

These are reviewed test scenarios, not a claim that the private suites passed in this review or that every feature is enabled in the current deployment.

## Additional experience

Lucas has reported experience with **Go and hardware/software integration**. A dedicated public code sample or detailed project case study for those areas has not yet been added. They are not presented as verified production deployments in this portfolio.

## Scope

This portfolio is a technical introduction, not an independent security audit. It does not assert revenue, customer numbers, performance benchmarks or coverage percentages. Detailed evaluation should connect a specific source revision to the relevant environment and test run.
