# TOMC native plugin data handling

This statement covers the local TOMC MCP tools bundled in TOMC native plugin 0.1.1, using TOMC runtime 0.1.0. The publisher is Zipeng Wu. Source and issue-based support are available at https://github.com/ZipengWu365/TOMC-plugin.

## Information handled

The tools receive only their supplied arguments: history, the next task, memory budgets, notebook names and any text the user authorizes the assistant to save. Supplied text may include personal or confidential information. TOMC does not automatically capture chats, scan personal files or intercept model API requests.

`prepare_context` computes memory locally and does not save the supplied history in a notebook. The server creates its local SQLite database and schema at startup. Optional `remember_memory` stores the supplied history under the chosen notebook name. `recall_memory` reads that notebook and returns prepared memory; `list_memories` returns names, counts and update times.

## Storage, retention and deletion

The database defaults to `~/.tomc/memory.sqlite3`, outside the plugin installation, or the location a host sets through `TOMC_MEMORY_PATH`. Clients using the same path share notebooks. TOMC does not encrypt the SQLite database. Protect access through your operating system and avoid saving sensitive material unless appropriate for your use.

There is no automatic time-based expiry. Saved history remains until `forget_memory` deletes the exact named notebook or you remove the local database file. Uninstalling the plugin does not remove notebooks. Tool deletion is logical database deletion, not a guarantee of secure erasure from storage devices, SQLite free pages, backups or copies. Backups and operating-system retention are controlled by you.

## Network and the host assistant

The MCP server runs on CPU over local stdio, exposes no HTTP listener, and makes no external model calls. This tool entry point does not send supplied history to a separate third-party service and contains no telemetry collection. Installation or first launch can access the internet to obtain the Python runtime and dependencies specified by `uv.lock`; later dependency changes require a plugin update.

Prepared memory and recalled content are returned to the host assistant. Your Claude or other host provider processes that output under its own account settings, terms and privacy policy. TOMC does not change those settings or remove material already in the host conversation. Do not assume that local preparation keeps an assistant's conversation local.

## Questions and changes

For technical questions or a reported problem, open an issue at https://github.com/ZipengWu365/TOMC-plugin/issues without including private history, credentials or notebook contents. This policy is shipped with the corresponding plugin version; substantive runtime/data-handling changes require an updated policy and release.
