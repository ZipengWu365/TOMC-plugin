# Native plugin validation — 2026-10-11

Plugin packaging version 0.1.1 contains the unchanged TOMC runtime 0.1.0.
The 35 imported runtime, license and lock files match the published MCPB byte for byte.
The plugin has no bundled private notebooks, credentials, model keys or executables.

## Checks completed

- Claude Code 2.1.296: `claude plugin validate --strict .claude-plugin/plugin.json` passed.
- The ZIP was relocated to a path containing spaces and Chinese characters. Its `.mcp.json` was exercised with the declared locked UV launch and an isolated dependency cache, environment and synthetic SQLite file.
- All five MCP tools were discovered and exercised. Checks included short Chinese text, default and explicit budgets, persistence across two server processes, notebook separation, exact deletion and a database kept outside the plugin folder.
- Five positive and three negative JSON-RPC MCP cases passed on the same runtime. Cases covered preparation without notebook saving, authorized saving and append, restart and recall, metadata listing, exact deletion, absent notebooks, malformed input, and treating a wildcard notebook name literally.
- Existing focused runtime, store, input, setup and packaging checks passed: 108 tests, with one POSIX-only permissions check skipped on Windows.

All of these checks used synthetic input. No paid model calls, personal notebook data or global assistant installation settings were used.

## Limits

WorkBuddy manual local MCP configuration was added on 2026-10-12, using an explicit STDIO template and the unchanged locked runtime. WorkBuddy client tool discovery, model calls and cross-session use have not been checked. This does not add a Tencent marketplace listing or establish Enterprise-client support.

Cursor and TRAE manual MCP configuration was added on 2026-10-11. Their client tool discovery and model calls have not been checked. The new template uses this repository's existing locked runtime; it does not add a native Cursor or TRAE plugin or a public directory listing.

These are package and tool-runtime checks. They do not establish model-selected tool behavior, quality or savings on every task, clean native GUI installation, Anthropic portal acceptance, official review, or public directory listing. The plugin is a candidate for that review. Earlier host/model checks are documented separately in the [main TOMC project](https://github.com/ZipengWu365/TOMC/blob/main/docs/validation.md).

## Local verification

Run `claude plugin validate --strict .claude-plugin/plugin.json`, then use `claude --plugin-dir .` and inspect `/mcp`. UV must be installed and available on the PATH used by Claude Code. First launch can download Python and locked dependencies. Use disposable synthetic notebooks for save/delete checks.
