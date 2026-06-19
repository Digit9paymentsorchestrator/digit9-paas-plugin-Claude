---
name: d9-setup
description: Explain how to configure the digit9-paas plugin and its digit9-sandbox MCP server. Triggers on setup, install, configuration, credentials, env vars, "MCP not working", "tools failing", D9_CLIENT_SECRET, sandbox access, or any question about what the plugin needs to run. Clarifies which features work without credentials and which require Digit9-issued sandbox secrets.
---

# Setting up the Digit9 PaaS plugin

The plugin runs at two tiers. Make the tier distinction explicit before asking the
user for any secret — most of the plugin needs nothing.

## Tier 1 — no credentials (works for everyone)

These need no configuration:

- Reference skills: `d9-auth`, `d9-master-data`, `d9-quote`, `d9-transaction`,
  `d9-status`, `d9-webhooks`, `d9-onboard`.
- Commands: `/digit9-paas:d9-scaffold`, `/digit9-paas:d9-validate`.
- Starter templates (Java/Spring, Node/TypeScript).

If the user only wants API guidance, scaffolding, or a code review, do not ask for
credentials — proceed.

## Tier 2 — Digit9 sandbox credentials (contracted partners only)

The `digit9-sandbox` MCP server and the `/digit9-paas:d9-auth-check` /
`/digit9-paas:d9-test` workflows make live sandbox calls and require credentials
that Digit9 issues to onboarded partners. Required, exported in the shell that
launches Claude Code (restart the session after exporting):

`D9_CLIENT_ID`, `D9_CLIENT_SECRET`, `D9_USERNAME`, `D9_PASSWORD`, `D9_SENDER`,
`D9_COMPANY`, `D9_BRANCH` (and optional `D9_WEBHOOK_SECRET`). `D9_BASE_URL` and
`D9_CHANNEL` are preset by the plugin — do not set them.

**If the MCP tools return an auth error and the user has no Digit9 credentials,
that is expected, not a bug.** Point them to their Digit9 integration manager or
<https://developer.digitnine.com>, and confirm Tier 1 still covers their need.

Never print or echo secret values. After setup, verify with
`/digit9-paas:d9-auth-check`. See `SETUP.md` at the repo root for the full table.
