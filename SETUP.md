# Setup

The `digit9-paas` plugin works at two tiers. **Most of it needs no credentials.**

## Tier 1 — no credentials required (works for everyone)

These load and run with nothing configured:

- **Reference skills** — `d9-auth`, `d9-master-data`, `d9-quote`, `d9-transaction`,
  `d9-status`, `d9-webhooks`, `d9-onboard`. Canonical API guidance with Node and
  Java code samples.
- **Slash commands** — `/digit9-paas:d9-scaffold`, `/digit9-paas:d9-validate`
  (scaffolding and code review need no live API).
- **Starter templates** — Java/Spring and Node/TypeScript projects copied into your
  workspace by `/digit9-paas:d9-scaffold`.

Install and use Tier 1 with no further setup:

```shell
/plugin marketplace add Digit9paymentsorchestrator/digit9-paas-plugin-Claude
/plugin install digit9-paas@digitnine
```

(Once the plugin is listed in the community directory you can instead use
`/plugin marketplace add anthropics/claude-plugins-community` then
`/plugin install digit9-paas@claude-community`.)

## Tier 2 — Digit9 sandbox credentials (contracted partners only)

The bundled `digit9-sandbox` MCP server makes **live calls to the Digit9 PaaS
sandbox** and the `/digit9-paas:d9-auth-check` and `/digit9-paas:d9-test` workflows
depend on it. It needs credentials that Digit9 issues to onboarded partners.

> **Without these credentials the MCP tools fail to authenticate. That is by
> design.** If you are not a contracted Digit9 partner, Tier 1 is the whole
> experience — nothing crashes, the MCP tools simply return an auth error.

### Required environment variables

Export these in the shell that launches Claude Code (the MCP server inherits the
process environment; a project `.env` alone does not update an already-running
session — restart after exporting):

| Variable | Notes |
| --- | --- |
| `D9_CLIENT_ID` | Issued by Digit9 (sandbox default often `cdp_app`) |
| `D9_CLIENT_SECRET` | Secret — never commit |
| `D9_USERNAME` | Issued by Digit9 |
| `D9_PASSWORD` | Secret — never commit |
| `D9_SENDER` | Partner sender code |
| `D9_COMPANY` | Partner company code |
| `D9_BRANCH` | Partner branch code |
| `D9_WEBHOOK_SECRET` | Optional — only for `d9_simulate_webhook` |

`D9_BASE_URL` (`https://drap-sandbox.digitnine.com`) and `D9_CHANNEL` (`Direct`) are
preset by the plugin manifest; you do not set them.

### Getting credentials

Sandbox credentials are provisioned during partner onboarding. Contact your Digit9
integration manager or see <https://developer.digitnine.com>. Never commit secrets;
keep them in your shell environment or an untracked `.env`.

### Verify

After exporting the variables and restarting Claude Code:

```
/digit9-paas:d9-auth-check
```

A green result means the MCP server reached the sandbox and the four context
headers (sender, channel, company, branch) are correct.
