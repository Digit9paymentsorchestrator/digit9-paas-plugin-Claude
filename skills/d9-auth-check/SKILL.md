---
name: d9-auth-check
description: Verify Digit9 PaaS sandbox credentials and required request headers through the bundled MCP server. Use only when the user explicitly asks to test authentication, connectivity, credentials, or sandbox setup.
---

# Check Digit9 Sandbox Authentication

Validate credentials without exposing secrets or bypassing the bundled MCP server.

## Preconditions

The Claude Code process must inherit `D9_CLIENT_ID`, `D9_CLIENT_SECRET`, `D9_USERNAME`, `D9_PASSWORD`, `D9_SENDER`, `D9_COMPANY`, and `D9_BRANCH`. The plugin supplies `D9_BASE_URL=https://drap-sandbox.digitnine.com` and `D9_CHANNEL=Direct`; `D9_WEBHOOK_SECRET` is optional.

If a required variable is missing, list only its name and stop. A project `.env` file alone does not update an already-running Claude Code session; tell the user to export the variables and restart the session.

## Authenticate

Call `d9_get_token` with no arguments. Never display the access token, client secret, or password. On success, report:

- sandbox base URL
- client ID and username
- token type and remaining token/refresh TTL
- sender, channel, company, and branch

Classify failures:

| Failure | Likely cause | Action |
| --- | --- | --- |
| `invalid_grant` | Username or password | Re-enter credentials issued by Digit9 |
| `invalid_client` | Client ID or secret | Confirm the issued client credentials |
| timeout or DNS | Network or base URL | Check network, VPN, DNS, and proxy |
| TLS error | Corporate interception | Configure the corporate CA or use an approved network |
| missing env var | Claude Code started before export | Export variables and restart the session |

## Verify context headers

Call `d9_get_banks` with `country="IN"` and `mode="BANK"`. A successful response confirms the shared client injected `sender`, `channel`, `company`, and `branch`. If Digit9 returns `40000 BAD_REQUEST`, identify the related variable names to recheck without echoing their values.

End with a compact pass/fail summary. Recommend `/digit9-paas:d9-test` only after both authentication and the bank lookup pass.
