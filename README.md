# Digit9 PaaS Integration Plugin

A Claude Code plugin for Licensed Financial Institutions integrating Digit9's Payments-as-a-Service cross-border remittance API.

## What it includes

- Canonical skills for authentication, onboarding, master data, quotes, transactions, status, and webhooks.
- Explicit scaffold, authentication-check, sandbox-test, and validation workflows.
- A bundled MCP server exposing eight typed Digit9 sandbox tools.
- Java/Spring Boot and Node.js/TypeScript partner starter projects.

## Install

From the community plugin directory (once listed):

```bash
/plugin marketplace add anthropics/claude-plugins-community
/plugin install digit9-paas@claude-community
```

Or directly from this repository:

```bash
/plugin marketplace add Digit9paymentsorchestrator/digit9-paas-plugin-Claude
/plugin install digit9-paas@digitnine
```

Use `/digit9-paas:d9-scaffold`, `/digit9-paas:d9-auth-check`, `/digit9-paas:d9-test`, and `/digit9-paas:d9-validate`. The seven API guidance skills also load automatically when relevant. See [SETUP.md](SETUP.md) for what works with and without Digit9 credentials.

## Credentials

The bundled MCP forwards these variables from the environment that launches Claude Code:

```text
D9_CLIENT_ID
D9_CLIENT_SECRET
D9_USERNAME
D9_PASSWORD
D9_SENDER
D9_COMPANY
D9_BRANCH
D9_WEBHOOK_SECRET       # optional
```

The plugin fixes `D9_BASE_URL` to `https://drap-sandbox.digitnine.com` and defaults `D9_CHANNEL` to `Direct`. Export credentials before starting Claude Code; adding them to `.env` after the session starts does not update the running MCP process.

## Coverage

The v0.1 plugin covers OAuth2 authentication, hosted KYC onboarding, corridors and bank masters, quote TTL handling, C2C/B2B transaction creation, confirmation and polling, and signed webhook processing.

## Support

- API documentation: <https://developer.digitnine.com>
- Sandbox access: contact your Digit9 integration manager
- Plugin issues: use the official Digit9 plugin repository once published

## License

Apache-2.0. See [LICENSE](LICENSE) and [NOTICE](NOTICE).
