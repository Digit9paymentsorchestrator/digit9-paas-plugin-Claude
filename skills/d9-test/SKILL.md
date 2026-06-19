---
name: d9-test
description: Run the live Digit9 PaaS sandbox happy path through the bundled MCP tools, covering authentication, masters, quote, transaction creation, confirmation, polling, and optional webhook simulation. Use only when the user explicitly requests the end-to-end sandbox test.
---

# Run the Digit9 Sandbox Happy Path

This workflow creates and confirms a real sandbox transaction. Explicit invocation is authorization for one sandbox run, never for production.

## Preconditions

1. Run `/digit9-paas:d9-auth-check`; stop unless authentication and headers pass.
2. Read `CLAUDE.md` for the configured corridor and service type. Default to `AE→IN BANK` and C2C if absent.
3. Confirm the `digit9-sandbox` tools are available.

Use these synthetic, sandbox-only fixtures (fictional identities — not real persons, and not live credentials):

- C2C sender: John Smith, `+971501234567`, AE, Emirates ID `784199191427626`, `id_code=4`.
- B2B sender: Acme Trading LLC, `agent_customer_number=TEST_BIZ_42`.
- IN BANK receiver: Priya Sharma, `+919812345678`, AADHAAR `1234-5678-9012`, HDFC Bank, account `12345678901234`, `account_type_code=1`.

## Execute in order

Record a pass/fail marker, latency, and returned non-secret identifiers for every step. Stop after the first failure.

1. Call `d9_get_token`.
2. Call `d9_get_corridors`, then `d9_get_banks(country="IN", mode="BANK")`.
3. Call `d9_quote` for AED 100 to INR by BANK. Capture `quote_id`, rate, fees, and expiry.
4. Read the `d9-transaction` skill in full, then call `d9_create_txn` with its canonical Postman-derived shape. Do not invent fields or send `agent_transaction_ref_number`.
5. Capture the 16-character `transaction_ref_number` and call `d9_confirm_txn`.
6. Call `d9_enquire_txn` every 5–10 seconds until COMPLETED, FAILED, or CANCELLED, with a five-minute cap. `IN_PROGRESS/TXN_PREPARED` for 60–120 seconds is normal.
7. If `D9_WEBHOOK_SECRET` and a reachable receiver are available, call `d9_simulate_webhook`; otherwise mark this step skipped.

## Diagnose failures

| Failure | Diagnosis |
| --- | --- |
| 401 | Re-run `/digit9-paas:d9-auth-check` and correct credentials |
| `40000 BAD_REQUEST` | Check context headers and the response `details` map |
| `40004 NOT_FOUND` during create | The ten-minute quote expired; obtain a new quote and require user confirmation before continuing |
| `806500` | Report every field reason from `errors[]` or `details` |
| Enquire timeout | Report sandbox latency; do not retry indefinitely |

## Report

Show corridor, service type, transaction reference, total duration, each step result, and the final state. Never print credentials or tokens. Do not claim success when the webhook step failed; distinguish failure from an intentional skip.
