---
name: d9-validate
description: Review partner integration code against Digit9 PaaS authentication, quote, transaction, status, and webhook requirements. Use only when the user explicitly asks to validate, audit, review, or assess a Digit9 integration.
---

# Validate a Digit9 Integration

Review only; do not rewrite code unless the user separately asks for fixes.

## Scope

Inspect files that call Digit9, shared API clients, webhook handlers, related configuration, and tests. Search common locations such as `src/d9/`, `src/integrations/digit9/`, and `src/main/java/com/partner/d9/`. Do not report unrelated business logic.

Read the relevant installed Digit9 skills before judging each surface. When code and a skill disagree with the onboarding Postman collection, the collection wins.

## Checks

Classify each concrete issue as BLOCKER, WARNING, or SUGGESTION.

### Authentication

- BLOCKER: credentials are hardcoded, tokens are logged, or required context headers are absent.
- BLOCKER: tokens are fetched per request rather than cached.
- WARNING: token refresh has no 30-second safety margin.
- WARNING: headers are duplicated per call instead of injected by one client/interceptor.

### Master data and quotes

- WARNING: corridors or banks are hardcoded, or master data is fetched per transaction.
- BLOCKER: money uses binary floating point.
- BLOCKER: an expired quote is silently replaced without user confirmation.
- WARNING: quote expiry is not surfaced to users or checked server-side.

### Transactions

- BLOCKER: `agent_transaction_ref_number` is sent in the canonical create body; Digit9 expects `sender.agent_customer_number` as the partner idempotency key.
- BLOCKER: the system `transaction_ref_number` is not stored for confirm, enquire, and reconciliation.
- BLOCKER: BANK details omit the required country-specific `account_type_code`, routing code, account number, or IBAN.
- BLOCKER: C2C and B2B sender fields are mixed.
- WARNING: actionable `details` or `errors[]` content is discarded.

### Status and webhooks

- BLOCKER: polling has no backoff or timeout cap.
- WARNING: the implementation relies only on webhooks and has no reconciliation backfill.
- BLOCKER: HMAC is computed over parsed/re-serialized JSON instead of raw body bytes.
- BLOCKER: signature failure returns 200 instead of 401.
- BLOCKER: replay protection or idempotency-key deduplication is missing.
- WARNING: webhook processing is slow or acknowledges before durable processing.

## Report

Return one Markdown report containing:

1. Project name and reviewed file count.
2. Blockers, warnings, and suggestions in separate sections.
3. For each finding: file, tight line reference, evidence, impact, one-line fix, and the relevant Digit9 skill.
4. A short “What looks good” section.
5. The single highest-value next step.

Do not inflate severity, repeat entire skills, expose secret values, or flag boilerplate merely for style.
