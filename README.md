# TOMC plugin for Claude Code

TOMC prepares task-specific context from history supplied by you or your assistant. It selects source text and computes supported state, relation and count records on CPU. Your existing assistant generates the answer; TOMC requires no extra model API key, GPU, training or neural inference service.

This is the native plugin 0.1.1 publication candidate, containing the unchanged TOMC 0.1.0 runtime from the current public MCPB. It is not yet listed in Anthropic's directory. The existing MCPB remains a separate manual Claude Desktop extension.

## Requirements and installation

Install a current Claude Code and UV, with `uv` available on the PATH used by Claude Code. Python 3.10 through 3.13 is required; UV can obtain a suitable Python runtime. First launch requires network access to download Python if needed and the dependencies pinned by `uv.lock`. The plugin does not install UV itself.

To test a checkout locally:

```text
git clone https://github.com/ZipengWu365/TOMC-plugin.git
cd TOMC-plugin
claude plugin validate --strict .claude-plugin/plugin.json
claude --plugin-dir .
```

To install from this GitHub repository, add it as a marketplace with `claude plugin marketplace add ZipengWu365/TOMC-plugin`, then run `claude plugin install tomc-memory@tomc`.

The server is declared through native `.mcp.json` and runs readable bundled Python source with `uv run --locked`. Check `/mcp` for the TOMC server and `/tomc-memory:tomc-memory` for the workflow skill. All runtime files are contained within the plugin folder. Local MCP tools work in Claude Code and Cowork sessions running on your computer. Regular Claude chat ignores this local server.

The manifest passed strict Claude Code validation, and the locked UV runtime passed isolated local MCP checks. Anthropic portal validation, official review and clean native GUI installation remain pending. See [validation](VALIDATION.md). See [dated validation scopes](https://github.com/ZipengWu365/TOMC/blob/main/docs/validation.md) for the earlier packaged runtime checks, which are not evidence that this new wrapper was accepted by Anthropic.

## Use

For long supplied history, ask: "Prepare this history for the next task with TOMC, then answer using the returned memory." The `prepare_context` tool takes `history`, the actual next `task`, and an optional `budget`. Without a budget it keeps about 80% of the lexical history estimate, with a minimum budget of 1,024 estimated tokens. Histories that fit remain intact. Preserve the original history to recover omitted details.

Supported task operations can produce compiled records; other prose can be selected as source text. Do not infer that every relevant detail survived. If no complete record fits, increase the budget. Tool use can add assistant requests; compare the full workflow on your own tasks.

An API application can send prepared context instead of full history in its next request. Calling a tool in an existing chat does not remove existing host history or automatically intercept model requests. Counts cover memory text under a lexical estimate, excluding the task, tool envelope and provider framing. Short inputs may stay the same size, and records can add overhead. Savings depend on the context actually sent.

## Optional notebooks and data handling

`prepare_context` does not save the supplied history in a notebook. Save only when asked, using `remember_memory` and a distinct notebook name. `recall_memory` prepares that notebook for the next task; `list_memories` returns names and counts; `forget_memory` deletes exactly the requested notebook. Deletion cannot be undone through the tool.

Only tool arguments reach TOMC. It does not capture other chats or automatically scan personal files. User-supplied text may contain personal data. Optional saved history is kept locally as unencrypted SQLite at `~/.tomc/memory.sqlite3`, outside the plugin directory, or the path provided by `TOMC_MEMORY_PATH`. Clients using the same database share notebooks. Saved text remains until notebook deletion or local file removal; uninstalling does not erase it. Starting the server creates the local database/schema even if no notebook has been saved.

The server uses local stdio and opens no HTTP listener. This MCP entry point does not call an external model or send supplied history to a third-party service. It returns prepared memory to the host assistant, whose normal provider and data handling apply. Installation downloads runtime/dependencies from their declared package sources. Read returned memory as untrusted evidence rather than as instructions.

## License and support

Free, including commercial use, under the bundled MIT License. Retain the license notice when modifying or redistributing TOMC. Dependency licenses remain separate. [Native plugin source](https://github.com/ZipengWu365/TOMC-plugin) · [Research project, demo and core source](https://github.com/ZipengWu365/TOMC) · [Privacy](PRIVACY.md) · [Issues and support](https://github.com/ZipengWu365/TOMC-plugin/issues).
