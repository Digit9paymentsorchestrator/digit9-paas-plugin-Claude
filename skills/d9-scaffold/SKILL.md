---
name: d9-scaffold
description: Bootstrap a Digit9 PaaS partner integration from the bundled Java/Spring or Node.js/TypeScript starter. Use only when the user explicitly asks to scaffold, initialize, or create a new Digit9 integration project.
---

# Scaffold a Digit9 PaaS Integration

Create a partner project from the plugin's canonical templates and leave it ready for sandbox authentication.

## 1. Protect existing work

Inspect the current directory before writing. If it already contains a substantial project, ask whether to use `./d9-integration/`; default to that subdirectory. Merge existing `.env`, `.env.example`, and `.gitignore` files instead of replacing them.

## 2. Collect project choices

Ask for:

1. Language: Java/Spring Boot 3 (Maven) or Node.js/TypeScript (ESM + Express).
2. Service type: C2C or B2B.
3. Default corridor: `AE→IN BANK`, `AE→PK BANK`, `AE→BD BANK`, `AE→PH CASHPICKUP`, `AE→LK BANK`, or `AE→NP BANK`.

## 3. Handle sandbox configuration safely

Required variables are `D9_CLIENT_ID`, `D9_CLIENT_SECRET`, `D9_USERNAME`, `D9_PASSWORD`, `D9_SENDER`, `D9_COMPANY`, and `D9_BRANCH`. `D9_WEBHOOK_SECRET` is optional. The bundled MCP supplies the sandbox base URL and `D9_CHANNEL=Direct`.

- Never print tokens or secret values.
- Write local values to `.env`, keep `.env` ignored, and create an empty `.env.example`.
- Write a project `.mcp.json` so Claude Code loads the `digit9-sandbox` MCP server (point its `args` at the plugin's `mcp/digit9-sandbox-server/dist/index.js`). See the `/digit9-paas:d9-scaffold` command for the exact file, the inlined env values, and the BOM-free write requirement.
- After writing `.mcp.json`, tell the partner to restart Claude Code and approve the project's MCP trust prompt before authentication can pass.

## 4. Render the starter

The plugin root is two levels above this skill directory.

- Copy `templates/java-spring/` for Java.
- Copy `templates/node-ts/` for Node.js.
- Replace `{{PROJECT_NAME}}`, `{{LANGUAGE}}`, `{{SERVICE_TYPE}}`, and `{{DEFAULT_CORRIDOR}}` in copied files.
- Render `CLAUDE.md.template` into the project root as `CLAUDE.md` with the same substitutions.

Preserve UTF-8 without a byte-order mark. Do not modify files outside the selected target directory.

## 5. Verify

Confirm the expected source tree, `.env.example`, `.gitignore`, `.mcp.json`, and `CLAUDE.md` exist. With `.mcp.json` in place and Claude Code restarted, call `d9_get_token` and then `d9_get_banks` with `country="IN"` and `mode="BANK"`. Otherwise report authentication as pending restart, not as a successful check.

For Java, note missing Java or Maven but leave the scaffold intact. For Node.js, note missing Node.js rather than installing it without permission.

## 6. Report

List the files created, the selected language/service/corridor, authentication status, and the next action. Recommend `/digit9-paas:d9-test` only after `/digit9-paas:d9-auth-check` succeeds. Remind the partner never to commit `D9_CLIENT_SECRET`, `D9_PASSWORD`, or `D9_WEBHOOK_SECRET`.
