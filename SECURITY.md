# Security Policy

## Reporting a vulnerability

If you discover a security issue in this plugin or its bundled `digit9-sandbox`
MCP server, please report it privately. **Do not** open a public GitHub issue for
security matters.

- Email: **business@lulufin.com** (subject line: `SECURITY — digit9-paas-plugin`)
- Or contact your Digit9 integration manager.

Please include the affected version/commit, reproduction steps, and impact. We aim
to acknowledge reports within 5 business days.

## Scope and data handling

- The plugin's skills, slash commands, and starter templates run locally and make
  no network calls on their own.
- The bundled `digit9-sandbox` MCP server talks only to first-party Digit9 hosts
  (`drap-sandbox.digitnine.com`, `drap-sbx.digitnine.com`). It does not call
  arbitrary or user-supplied hosts, except `d9_simulate_webhook`, which POSTs to a
  webhook URL **you** provide (your own receiver).
- Credentials (`D9_CLIENT_SECRET`, `D9_PASSWORD`, etc.) are read from the process
  environment, used only to obtain an OAuth2 token from Digit9, and are never
  written to disk or logged.
- No secrets are committed to this repository. `.env` is gitignored; `.env.example`
  ships with empty values.
- All example identities in the test workflows (names, Emirates ID, AADHAAR, phone
  numbers, account numbers) are **synthetic sandbox fixtures**, not real personal
  data.

## Supported versions

The latest published version on the `main` branch is supported. Fixes are released
forward; there is no long-term support branch.
