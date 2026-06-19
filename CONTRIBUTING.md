# Contributing

Thanks for helping improve the Digit9 PaaS plugin. This repo **is** the plugin —
editing files here changes how the assistant guides partners integrating Digit9's
PaaS cross-border remittance API.

## Ground rules

- **Source of truth for API shapes** is the Digit9 Postman collection. If a skill
  and the collection disagree, the collection wins. Don't invent field names from
  generic remittance docs — the sandbox rejects with `40000 BAD_REQUEST` naming the
  bad field.
- **Keep the three surfaces aligned.** When a Digit9 API shape changes, update all
  of them in the same change: the skill code sample (`skills/<name>/SKILL.md`), the
  MCP tool (`mcp/digit9-sandbox-server/src/tools/*` — `inputSchema` + request body),
  and the matching service in both templates (`templates/{node-ts,java-spring}/`).
- **Never commit secrets.** `.env` is gitignored; `.env.example` ships empty. All
  example identities in the test workflows are synthetic sandbox fixtures.

## Building the MCP server

The plugin has no top-level build; only the bundled MCP server compiles:

```bash
cd mcp/digit9-sandbox-server
npm install
npm run build      # tsc → dist/ (committed to the repo)
npm run typecheck  # tsc --noEmit
```

`dist/` is intentionally checked in so installs don't require a compile step. After
any change under `mcp/digit9-sandbox-server/src/`, rebuild and commit `dist/` in the
same change.

## Validating

Before opening a PR, run the plugin validator from the repo root:

```bash
claude plugin validate .
```

Fix any errors; warnings should be addressed where reasonable.

## Submitting changes

1. Branch from `main`.
2. Make the change, keeping the three surfaces aligned and `dist/` rebuilt.
3. Run `claude plugin validate .`.
4. Open a pull request describing what changed and why.

## Reporting security issues

Do not open a public issue for security matters — see [SECURITY.md](SECURITY.md).

## License

By contributing, you agree that your contributions are licensed under the
[Apache License 2.0](LICENSE), the same license as this project.
